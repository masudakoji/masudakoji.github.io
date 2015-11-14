---
layout: post
title: "python3でcasklistを作ろうとしたらエラーが出た"
lang: ja
category: ja
tags: [Python, Homebrew]
---
{% include JB/setup %}
{% include JB/langselect %}

``brew file casklist`` すると以下の様なエラーが出た。

	  File "/usr/local/bin/brew-file", line 107
	    except OSError, e:
	                  ^
	SyntaxError: invalid syntax

現在のPython環境はpyenvで入れたpython 3.3.2なのですが、``pyenv local 2.7.8``を実行してpython 2.7.8にしたら動きました。