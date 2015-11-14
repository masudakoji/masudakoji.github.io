---
layout: post
title: "Bokmarklet to create a link tag for Sphinx"
lang: en
category: en
tags: [Python, Sphinx, reST]
---
{% include JB/setup %}
{% include JB/langselect %}

## Sphinx

Sphinx is a documentation builder written in Python, which is used in the official website of Python and others. I use that for my research notes. Sphinx can generate HTML, PDF and docx documents from the source files of ReST-formatted text.


## Bokmarklet to create a link tag

When I want to add links to Sphinx documents, it's complicated to make reST-formatted link. I make the bookmarklet to create a link tag.

{% highlight javascript %}
javascript:var ret=window.prompt('LinkTagForSphinx','`'+document.title+'  <'+location.href+'>`_');
{% endhighlight %}

To use that, add the script as a new bookmark.