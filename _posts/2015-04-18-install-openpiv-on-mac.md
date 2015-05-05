---
layout: post
title: "OpenPIVをMacにインストールする"
description: ""
category: 
tags: [Mac, Python, PIV, Open PIV, Fluid Mechanics]
---
{% include JB/setup %}
<!-- Install OpenPIV on Mac -->

MacにオープンソースのPIV (Particle Image Velocimetry) ツールのOpenPIVをインストールしました。

pyenv環境 (version: 2.7.9)にpipでインストールしました。

まずはcythonが必要だとのことなので以下のようにインストール。

{% highlight bash%}
pip install cython
pip install openpiv
{% endhighlight %}

OpenPIVのインストール自体はすんだものの、pythonからimport openpivしようとしたらいくつかエラーが出たので解決します。

{% highlight bash%}
     13
     14 # import default modules
---> 15 import openpiv.tools
     16 import openpiv.pyprocess
     17 import openpiv.scaling

/Users/koji/.pyenv/versions/2.7.9/lib/python2.7/site-packages/openpiv/tools.py in <module>()
     31 import matplotlib.image as mpltimg
     32 from scipy import ndimage
---> 33 from skimage import filter, io
     34
     35

ImportError: No module named skimage
{% endhighlight %}

skimage (scikit-image) などが必要らしいのでインストールしましょう。
あとはprogressbarも必要だそうです。

{% highlight bash%}
pip install scikit-image
pip install progressbar
{% endhighlight %}

とすることでopenpivをpythonのモジュールとしてインポート出来ます。

しかし、同梱されているチュートリアルプログラムを実行しようとしたところ以下のエラーが出ます。

{% highlight bash%}
Traceback (most recent call last):
  File "./tutorial-part1.py", line 8, in <module>
    u, v, sig2noise = openpiv.process.extended_search_area_piv( frame_a, frame_b, window_size=24, overlap=12, dt=0.02, search_area_size=64, sig2noise_method='peak2peak' )
  File "piv/src/process.pyx", line 23, in openpiv.process.extended_search_area_piv (openpiv/src/process.c:2290)
ValueError: Buffer dtype mismatch, expected 'DTYPEi_t' but got 'unsigned char'
{% endhighlight %}

https://github.com/OpenPIV/openpiv-python/issues/35
でも言及されていますが、
どうやらオフィシャルのものにはチュートリアルプログラムにバグがあるようです。[alexlib](https://github.com/alexlib)によってforkされた[OpenPIV](https://github.com/alexlib/openpiv-python)ではこの問題が解決されているようです。

{% highlight bash%}
git clone git@github.com:alexlib/openpiv-python.git
python setup.py install
{% endhighlight %}

とするとalexlibのOpenPIVがインストールできます。
とはいえ、オフィシャルのプログラムもチュートリアルプログラムのバグを解消すれば正常に動作します。
それは[別の記事](../../../../2015/05/05/fixing-bugs-in-tutorial-program-of-openpiv/)で。
