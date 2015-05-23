---
layout: post
title: "GitリポジトリからLaTeXソースを取り出してLaTeXdiffにかけるプログラム"
lang: ja
category: ja
tags: [LaTeX, Python, Git]
---
{% include JB/setup %}
{% include JB/langselect %}

<!--Run LaTeXdiff from Git repository for separated LaTeX files-->

##はじめに
[LaTeXdiff](http://crypto.junod.info/2010/07/03/latexdiff/)というツールがあります。このツールを使うことで2つのLaTeXソースを比較してその差分を見やすく表示することが出来ます。

論文などをLaTeXで書く際にはGitなどでバージョン管理することがあるかと思います。その時の差分を取って表示することが出来たら便利だと思うので、処理するプログラムを書きました。

###やりたいこと
やりたいことは以下の通り

* GitのコミットIDを2つ指定するとその2つのコミットの差分をLaTeXdiffで調べて出力
* 単一のLaTeXソースじゃなくて分割したファイルをincludeする場合にも対応

なお、今回は英語の場合しか試してません。


##プログラムの概要

###必要環境
当然のことながら、TeX環境およびlatexdiffは必要です。latexdiffはMacTeXでインストールした場合には既にインストールされているはずです。

プログラムはPythonで書きましたので、Python環境も必要です。

###もともとのTeXソース
まず今回使ったメインのTeXファイル。``1.tex``、``2.tex``などをincludeしています。

{% highlight latex%}
\documentclass[a4paper,12pt,fleqn]{jsarticle}

\begin{document}

\include{0}
\include{1}
\include{2}
\include{3}
\include{4}

\end{document}
{% endhighlight %}

今回は``1.tex``、``2.tex``、``3.tex``、``4.tex``の4つのファイルについて差分を取ることにします。

###Gitリポジトリからファイルを取り出す
Gitリポジトリから任意のコミット時におけるファイルを開くには
{% highlight bash%}
git show a8ebb89:1.tex
{% endhighlight%}
のようにします。``a8ebb89``はコミットIDです。
結果は標準出力に出力されますのでリダイレクトするなりパイプに渡して使うことが出来ます。

###実行するPythonプログラム
{% highlight python linenos%}
#!/usr/bin/env python
#coding: utf-8
import sys
import shlex
import subprocess
FileList=['1.tex', '2.tex', '3.tex', '4.tex' ]

def write_source(input_file, output_file, commit_id):
   print('{0} -> {1} [{2}]'.format(input_file, output_file, commit_id))
   # Show source from Git
   cmd=shlex.split('git show {0}:{1}'.format(commit_id, input_file))
   with open(output_file, 'w') as f:
      proc=subprocess.Popen(cmd, stdout=f, stderr=subprocess.PIPE)
   stdout, stderr=proc.communicate()
   # errchk
   if stderr:
      print("standard error of subprocess:")
      print(stderr)
      sys.exit()


def generate_diff(input_file, old, new):
   # define names
   old_file='{0}_{1}'.format(old, input_file)
   new_file='{0}_{1}'.format(new, input_file)
   output_file='diff_{0}'.format(input_file)
   # latexdiff
   cmd=shlex.split('latexdiff {0} {1}'.format(old_file, new_file))
   with open(output_file, 'w') as f:
      proc=subprocess.Popen(cmd, stdout=f, stderr=subprocess.PIPE)
   stdout, stderr=proc.communicate()
   # errchk
   if stderr:
      print("standard error of subprocess:")
      print(stderr)


def clean(old, new):
   import glob
   import os
   print('Removing intermediate files...')
   for commit_id in [old, new]:
      files=glob.glob('{0}_*'.format(commit_id))
      for filename in files:
         os.unlink(filename)


def main(old, new):
   print('commit id(old): {0}'.format(old))
   print('commit id(new): {0}'.format(new))
   for input_file in FileList:
      for commit_id in [old, new]:
         output_file='{0}_{1}'.format(commit_id, input_file)
         write_source(input_file, output_file, commit_id)
      generate_diff(input_file, old, new)


if __name__ == '__main__':
   # reading arguments
   argvs=sys.argv
   argc=len(argvs)
   if argc != 3:
      print('This program takes 2 arguments.')
      print('Usage: python {0} commit id(old) commit id(new)'.format(argvs[0]))
      sys.exit()
   main(argvs[1], argvs[2])
   #clean(argvs[1], argvs[2])
{% endhighlight%}
上のプログラムを適当な名前をつけて保存してください。以下では``latexdiff.py``として保存したとして話を進めます。

###使い方
6行目のFileListに差分を取りたいLaTeXのソースファイルを記入します。

あとは
{% highlight bash%}
python latexdiff.py CommitID(old) CommitID(new)
{% endhighlight%}
を実行してください。

中間ファイルを削除するためにclean関数も定義してありますが、上のプログラムではコメントアウトしてあります。必要であれば使ってください。

###結果を表示するためのTeXソース
上のプログラムを実行すると、``diff_1.tex``のようなファイルが出力されます。
あとは、これらをインクルードするTeXファイルを作ってください。先ほどのメインファイルを少し修正すればいいでしょう。

{% highlight latex%}
\documentclass[a4paper,12pt,fleqn]{jsarticle}

\begin{document}

\include{0}
\include{diff_1}
\include{diff_2}
\include{diff_3}
\include{diff_4}

\end{document}
{% endhighlight%}

あとはこちらをコンパイルすれば、2つのコミット間の差が見やすく表示されるはずです。

###最後のコンパイルがうまくいかない場合
うまくコンパイルできない場合は以下をTeXソースのプリアンブルに記述するとうまくいくと思います。

{% highlight latex%}{% raw %}
\RequirePackage[normalem]{ulem}
\RequirePackage{color}\definecolor{RED}{rgb}{1,0,0}\definecolor{BLUE}{rgb}{0,0,1}
\providecommand{\DIFadd}[1]{{\protect\color{blue}\uwave{#1}}}
\providecommand{\DIFdel}[1]{{\protect\color{red}\sout{#1}}}\providecommand{\DIFaddbegin}{}
\providecommand{\DIFaddend}{}
\providecommand{\DIFdelbegin}{}
\providecommand{\DIFdelend}{}
\providecommand{\DIFaddFL}[1]{\DIFadd{#1}}
\providecommand{\DIFdelFL}[1]{\DIFdel{#1}}
\providecommand{\DIFaddbeginFL}{}
\providecommand{\DIFaddendFL}{}
\providecommand{\DIFdelbeginFL}{}
\providecommand{\DIFdelendFL}{}{% endraw %}
{% endhighlight%}

##おわりに
以上です。すでに似たようなことをされてる方もおられるますし、やってることは大したことではありませんが、LaTeXソースを分割してGitでバージョン管理してる人にはそこそこ便利だと思います。


####参考
* [gitで管理されているtexファイルの差分pdfを作成するスクリプト](http://qiita.com/kuranari_tm/items/73a276c3d1e4e2cf139e)
* [Error when trying to generate a PDF from a latexdiff document.](http://tex.stackexchange.com/questions/15121/error-when-trying-to-generate-a-pdf-from-a-latexdiff-document)
* [Using latexdiff with git](http://tex.stackexchange.com/questions/1325/using-latexdiff-with-git)
* [latexdiffを日本語LaTeX文書でうまく扱えるようにする](http://sky-y.hatenablog.jp/entry/2013/02/03/010723)