---
layout: post
title: "code sample"
description: ""
category: 
tags: [ruby, jekyll, GitHub]
---
{% include JB/setup %}

### Jekyllでシンタックスハイライトを使う
ちゃんとした？コードだったらGistを使ったほうが手っ取り早いのかも知れませんが。ちょっとしたコードを記事中に貼り付ける際に、シンタックスハイライトを行いたいので、その方法を。

シンタックスハイライトはpygmentsを使えばいいそうです。

最近のJekyll-Bootstrapだとすでに、`_config.yml`の中に`highlighter: pygments`の記述があるかと思います。以前のものだと`pygments: true`みたいな記述のようですが。

GitHubにはpygmentsは既に入っているみたいなので、テーマのhtmlファイルの中でcssを読み込む記述さえ入れておけば良い。

テーマ'twitter'ならば、`_includes/themes/twitter`の`default.html`の中に

{% highlight html %}
<link href="{{ ASSET_PATH }}/css/syntax.css?body=1" rel="stylesheet" type="text/css" media="all">
{% endhighlight %}

を追加しておく。そして当然css実物も必要ですので、

`assets/themes/twitter/css`の中にsyntax.cssを追加しておく。

記事中に貼り付ける際には以下のようにすると、

	{% highlight ruby linenos %}
	def foo
	  puts 'foo'
	end
	{% endhighlight %}

色分けされて表示されます。

{% highlight ruby linenos %}
def foo
  puts 'foo'
end
{% endhighlight %}

#### 参考
[Creating your Jekyll-Bootstrap powered blog for R blogging](http://lcolladotor.github.io/2013/11/09/new-Fellgernon-Bit-setup-in-Github/#.VDCwMCl_vgI)

