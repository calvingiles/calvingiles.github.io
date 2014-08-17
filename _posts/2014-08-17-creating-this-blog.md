---
layout: post
author: Calvin Giles
category: posts
title: Creating a github pages blog with Jekyll
---

After creating a new blog, it seems one must post a [hello world][hello], then follow it with a post on creating the blog site.

No doubt you have read a few of these before, so I will assume you knew what I did when I started, including what [Jekyll][jekyll] is and what [github pages][gh-pages] are.

I wanted to use a theme beyond the Jekyll standard, but I am not a web designer so finding an existing theme was required. One quick google later, a click on the top link [Jekyll Themes][jk-themes] and I was browsing a decent selection. I want my posts to include ipython [notebooks][ipynb] in the future, so I wanted a theme with a decent width for content (no huge sidebar). Also, I am not a designer and don't pretend to be - I wanted the theme to reflect this. I liked Zach Holman's [left][left], so forked it.

The customisation is very simple - owing to the good site structure Holman has put in place. I updated the _config.yaml and layout files and I was good to go.

I didn't like the default style for links (underlined) so I changed the css to this:

    {% highlight css %}
    a {
      color: #227ce8; 
      text-decoration: none; }
    a:hover {
      text-decoration: underline; }
    {% endhighlight %}

  

Now I only get an underline on hover.

Getting notebooks working as desired will happen when it needs to -- I will try to be [agile][agile] with this blog. If you want to see what this looks like, check out my [issues][issues] -- they are all public.

[hello]: /posts/hello-world/
[jekyll]: http://jekyllrb.com/
[jk-themes]: http://jekyllthemes.org/
[left]: https://github.com/holman/left
[gh-pages]: https://pages.github.com/
[ipynb]: http://ipython.org/notebook.html
[agile]: http://www.agileforall.com/2009/02/new-to-agile-remember-one-thing-just-enough-just-in-time/
[issues]: https://github.com/calvingiles/calvingiles.github.io/issues
