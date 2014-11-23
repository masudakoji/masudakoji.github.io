---
layout: post
title: "Sphinx（reST）用のリンクタグを出力するブックマークレット"
description: ""
category: 
tags: [Python, Sphinx, reST]
---
{% include JB/setup %}

## Sphinxとは

Sphinxとは、Pythonで書かれたドキュメントビルダーで、Pythonの公式サイトやその他のサイトで用いられています。私自身も、自分用の研究ノート・メモ帳として、活用しております。Sphinxでは記事はreST型式で記述し、コンパイルすることで、HTMLやPDF（設定すればdocxでも）で出力されます。

## リンクを作成するブックマークレット

インターネットをしている際に見つけたサイトをSphinxに載せようと思った時、いちいちリンクのタグをReST型式で記述するのは面倒なので、ネットで見つけた先人達の記事を参考にしてブックマークレットを作りました。

{% highlight javascript %}
javascript:var ret=window.prompt('LinkTagForSphinx','`'+document.title+'  <'+location.href+'>`_');
{% endhighlight %}

上のスクリプトを新しいリンクとして追加すれば使えます。