---
layout: post
title: "Jekyllでpygmentsを使っている際にコンパイルに失敗する"
description: ""
category: 
tags: [Python, Jekyll]
---
{% include JB/setup %}

## pygmentsはPython3に対応していない

以前の記事でも書いたように、Jekyllでシンタックスハイライトを使うために、pygmentsを有効にしているわけですが、環境によってはうまくコンパイルできませんでした。

`jekyll serve`をしようとしたら次のようなエラーが。

{% highlight bash%}
Generating...
  Liquid Exception: Failed to get header. in _posts/xxxxxxxx.md
{% endhighlight %}

pygmentsはPythonで作成されているみたいなので、Pythonのversionによってはエラーが出るみたい。

現在デフォルトのPythonは3.3.2にしてるのですが、pygmentsはPython3に対応してないためにエラーが出るそうです。

なので、pyenvでJekyllのディレクトリだけ、Pythonのバージョンを変更すれば解決。

{% highlight bash%}
pyenv local 2.7.8
{% endhighlight %}

やはり、デフォルトのPythonを3にするのは時期尚早なのか。

## 参考
[【Jekyll】での"Failed to get header."というエラーの対策](http://totora0155.hatenablog.jp/entry/2013/08/14/030820)
