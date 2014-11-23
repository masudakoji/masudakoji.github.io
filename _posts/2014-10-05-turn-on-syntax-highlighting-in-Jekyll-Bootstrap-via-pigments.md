---
layout: post
title: "Jekyll-Bootstrapでpigmentsを使ってシンタックスハイライト表示するには"
description: ""
category: 
tags: [Ruby, Jekyll, GitHub]
---
{% include JB/setup %}

## はじめに
ちゃんとした？コードだったらGistを使ったほうが手っ取り早いのかも知れませんが。ちょっとしたコードを記事中に貼り付ける際に、シンタックスハイライトを行いたいので、その方法を。

シンタックスハイライトはpygmentsを使えばいいそうです。

最近のJekyll-Bootstrapだとすでに、`_config.yml`の中に`highlighter: pygments`の記述があるかと思います。以前のものだと`pygments: true`みたいな記述のようですが。

GitHubにはpygmentsは既に入っているみたいなので、テーマのhtmlファイルの中でcssを読み込む記述さえ入れておけば良い。

## cssを読み込む
テーマ'twitter'ならば、`_includes/themes/twitter`の`default.html`の中に

{% highlight html %}{% raw %}
<link href="{{ ASSET_PATH }}/css/syntax.css?body=1" rel="stylesheet" type="text/css" media="all">{% endraw %}
{% endhighlight %}

を追加しておく。そして当然css実物も必要ですので、

`assets/themes/twitter/css`の中にsyntax.cssを追加しておく。

記事中に貼り付ける際には、`{% raw %}{% highlight ruby linenos%}{% endraw %}`と`{% raw %}{% endhighlight %}{% endraw %}`で囲んでコードを記述すると、以下のように色分けされて表示されます。

{% highlight ruby linenos %}
def foo
  puts 'foo'
end
{% endhighlight %}

### 参考
[Creating your Jekyll-Bootstrap powered blog for R blogging](http://lcolladotor.github.io/2013/11/09/new-Fellgernon-Bit-setup-in-Github/#.VDCwMCl_vgI)

[How to escape liquid template tags?](http://stackoverflow.com/questions/3426182/how-to-escape-liquid-template-tags)

[jekyllのブログで投稿一覧ページにtwitterのボタンを置く](http://imaizu.me/program/twbtn-on-jekyll-post.html)

余談ですが、jekyllの括弧のエスケープにはまっていました。