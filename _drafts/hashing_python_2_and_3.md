# Stable hashing in python 2 and 3

In order to save myself some headaches with sql, I implemented a very lazy cache of data retrieved from my database:

```python
def get_sql_data(sql_str, connection_or_sql_al_engine, force_reload_data=False):
    """
    Return a dataframe containing the result of calling 'sql_str' on 'connection'.

    As side effect, store query result in local folder with year, week in year and 'sql_str' hash.
    If cache already exists for this query and week, just return the cache.
    """
    hash_str = '{:x}'.format(abs(hash(sql_str)))
    date_str = pandas.datetime.today().strftime('%Y-%W')
    fname_str = expanduser('~/data/sql_cache/{}-{}.csv.sql_cache'.format(date_str, hash_str))
    try:
        if force_reload_data:
            raise IOError()
        return pandas.read_csv(fname_str, encoding='utf-8')
    except IOError:
        df = pandas.read_sql_query(sql_str, connection_or_sql_al_engine)
        df.to_csv(fname_str, encoding='utf-8', index=False)
        return df
```

The first time you run a sql query, the data is collected from the server and stored locally in a cache directory. If you repeat the query within the same calendar week, the data is simply fetched from the cache. This is useful because it means I can work on prototypes all week without having to wait for the same data to download. I usually keep some data in my ipython session but without a cache I would have to be careful to avoid restarting the kernal. This creates bad habbits (bad code execution order not notices for one) so I prefer an approach that makes restarting trivial.

One downside is that _any_ change to the sql string causes a reload. This includes indentation, such as including in for loops, but this is not too much of a headache.

In python 2, this does what you expect - Load some data (slow), reload (fast) then restart the kernel and reload (fast). In python 3, however, the last reload is now slow. This is due to python randomizing the hash function on start. Not a problem for most purposes, but a significant problem for my purposes as it eliminates all gain for caching! (Note, if you only want same session caching and are using python 3, look at `functools.lru_cache` as this is an excellent implementation and part of the std library.

To solve this, I went to the builtin `hashlib` package. Simply changing line 8 for `hash_str = hashlib.md5(sql_str.encode('utf-8')).hexdigest()` solves the problem. The whole function is now:

```python
import hashlib
import pandas

def get_sql_data(sql_str, connection_or_sql_al_engine, force_reload_data=False):
    """
    Return a dataframe containing the result of calling 'sql_str' on 'connection'.

    As side effect, store query result in local folder with year, week in year and 'sql_str' hash.
    If cache already exists for this query and week, just return the cache.
    """
    hash_str = hashlib.md5(sql_str.encode('utf-8')).hexdigest()
    date_str = pandas.datetime.today().strftime('%Y-%W')
    fname_str = '/data/sql_cache/{}-{}.csv.sql_cache'.format(date_str, hash_str)
    try:
        if force_reload_data:
            raise IOError()
        return pandas.read_csv(fname_str, encoding='utf-8')
    except IOError:
        df = pandas.read_sql_query(sql_str, connection_or_sql_al_engine)
        df.to_csv(fname_str, encoding='utf-8', index=False)
        return df
```
