---
layout: post
title: "Error on brew file"
lang: en
category: en
tags: [Python, Homebrew]
---
{% include JB/setup %}
{% include JB/langselect %}

I got below error executing ``brew file casklist``.

	  File "/usr/local/bin/brew-file", line 107
	    except OSError, e:
	                  ^
	SyntaxError: invalid syntax

My Python environmnet is 3.3.2 (provided using pyenv).
I could fix the error by proceeding ``pyenv local 2.7.8``.