---
layout: post
title: "Failed to get header at jekyll build"
lang: en
category: en
tags: [Python, Jekyll]
---
{% include JB/setup %}
{% include JB/langselect %}

## pygments is not supporting Python3

To use syntax highlighting on Jekyll, I use pygment Plug-in. I got this error when compiling.

When I'm trying to compile with `jekyll serve`,  it results.

{% highlight bash%}
Generating...
  Liquid Exception: Failed to get header. in _posts/xxxxxxxx.md
{% endhighlight %}

pygments is written in Python, but Python3 is not supported. (I set Python 3.3.2 as default.)

To fix it, set the default Python only the local directory with pyenv.

{% highlight bash%}
pyenv local 2.7.8
{% endhighlight %}

is it premature to set Pyththon3 as default??

## Reference
[【Jekyll】での"Failed to get header."というエラーの対策](http://totora0155.hatenablog.jp/entry/2013/08/14/030820)