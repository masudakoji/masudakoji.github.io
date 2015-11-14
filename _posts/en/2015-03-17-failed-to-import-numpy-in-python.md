---
layout: post
title: "Failed to import numpy in Python"
lang: en
category: en
tags: [Python, numpy]
---
{% include JB/setup %}
{% include JB/langselect %}


## numpyがimport出来ない

先日まで動いていたのですが、
iPythonからnumpyをimportしようとしたら急に以下の様なエラーが出るようになった。

{% highlight bash%}
In [1]: import numpy
---------------------------------------------------------------------------
ImportError                               Traceback (most recent call last)
<ipython-input-1-5a0bd626bb1d> in <module>()
----> 1 import numpy

/tmp/pip-build-K9_8uY/numpy/numpy/__init__.py in <module>()

/tmp/pip-build-K9_8uY/numpy/numpy/add_newdocs.py in <module>()

/tmp/pip-build-K9_8uY/numpy/numpy/lib/__init__.py in <module>()

/tmp/pip-build-K9_8uY/numpy/numpy/lib/type_check.py in <module>()

/tmp/pip-build-K9_8uY/numpy/numpy/core/__init__.py in <module>()

ImportError: cannot import name multiarray
{% endhighlight %}

当然matplotlibを使った可視化も出来なくなっている。
homebrewの方のnumpyをアップデートした時に何か問題が起きたか、pythonの他のモジュールと競合がおこるようなったかでしょうか。

当方のpython環境は、pyenvを使って構築したpython2.7.9。numpyはpipで導入したもので、バージョンは1.9.1。

他のversionだったらどうだろうと思ってpyenv で以前作ったpython 3.3.6で試してみると問題なくimport 出来る。numpyのバージョンは同じく1.9.1。
他のモジュールをインストールしたのがきっかけでズレた？pipで入れたOpenPIVとかhomebrewで入れたopencvあたりが怪しい気もする。


## pip でnumpyを再度インストール

とりあえずpipでnumpyをインストールし直そうとしたら

{% highlight bash%}
Exception:
Traceback (most recent call last):
  File "/Users/USERNAME/.pyenv/versions/2.7.9/lib/python2.7/site-packages/pip/basecommand.py", line 232, in main
    status = self.run(options, args)
  File "/Users/USERNAME/.pyenv/versions/2.7.9/lib/python2.7/site-packages/pip/commands/uninstall.py", line 70, in run
    requirement_set.uninstall(auto_confirm=options.yes)
  File "/Users/USERNAME/.pyenv/versions/2.7.9/lib/python2.7/site-packages/pip/req/req_set.py", line 152, in uninstall
    req.uninstall(auto_confirm=auto_confirm)
  File "/Users/USERNAME/.pyenv/versions/2.7.9/lib/python2.7/site-packages/pip/req/req_install.py", line 667, in uninstall
    paths_to_remove.remove(auto_confirm)
  File "/Users/USERNAME/.pyenv/versions/2.7.9/lib/python2.7/site-packages/pip/req/req_uninstall.py", line 126, in remove
    renames(path, new_path)
  File "/Users/USERNAME/.pyenv/versions/2.7.9/lib/python2.7/site-packages/pip/utils/__init__.py", line 316, in renames
    shutil.move(old, new)
  File "/Users/USERNAME/.pyenv/versions/2.7.9/lib/python2.7/shutil.py", line 303, in move
    os.unlink(src)
OSError: [Errno 13] Permission denied: '/usr/local/lib/python2.7/site-packages/numpy-1.9.1.dist-info/DESCRIPTION.rst'
{% endhighlight %}

というエラーがでた。仕方がないのでrootで

{% highlight bash%}
sudo pip uninstall numpy
pip install numpy
{% endhighlight %}

としたら無事numpy (1.9.2) がインストールされた。importも出来るようになったのでとりあえず解決。
