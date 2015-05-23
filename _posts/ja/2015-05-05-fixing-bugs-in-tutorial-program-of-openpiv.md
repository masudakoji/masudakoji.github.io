---
layout: post
title: "OpenPIVのチュートリアルプログラムのバグ"
lang: ja
category: ja
tags: [Mac, Python, PIV, Open PIV, Fluid Mechanics]
---
{% include JB/setup %}
{% include JB/langselect %}

<!--Fixing bugs in tutorial program of OpenPIV-->

[前回の記事](../../../../2015/04/18/install-openpiv-on-mac/)でオフィシャルのOpenPIVに同梱されているチュートリアルプログラムにバグがあるといいました。

[alexlibのOpenPIV](https://github.com/alexlib/openpiv-python)を使えば動くのですが、オフィシャルのチュートリアルプログラムもバグを直せば動作します。

Googleグループに[このバグに関する記事](https://groups.google.com/forum/#!topic/openpiv-users/FHc0pcmEjyE)が投稿されていました。
画像をインポートするときにフォーマットを指定するのが重要なようです。

ちなみに正常に動作しなかった（オフィシャルのOpenPIVにあった）プログラムは以下の通り。

{% highlight python%}
import openpiv.tools
import openpiv.process
import openpiv.scaling


frame_a  = openpiv.tools.imread( 'exp1_001_a.bmp' )
frame_b  = openpiv.tools.imread( 'exp1_001_b.bmp' )

u, v, sig2noise = openpiv.process.extended_search_area_piv( frame_a, frame_b, window_size=24, overlap=12, dt=0.02, search_area_size=64, sig2noise_method='peak2peak' )

x, y = openpiv.process.get_coordinates( image_size=frame_a.shape, window_size=24, overlap=12 )

u, v, mask = openpiv.validation.sig2noise_val( u, v, sig2noise, threshold = 1.3 )

u, v = openpiv.filters.replace_outliers( u, v, method='localmean', max_iter=10, kernel_size=2)

x, y, u, v = openpiv.scaling.uniform(x, y, u, v, scaling_factor = 96.52 )

openpiv.tools.save(x, y, u, v, mask, 'exp1_001.txt' )

openpiv.tools.display_vector_field('exp1_001.txt', scale=100, width=0.0025)
{% endhighlight %}

正常に動作したのがこちら。

{% highlight python%}
import openpiv.tools
import openpiv.process
import openpiv.scaling
import numpy as np

frame_a  = openpiv.tools.imread( 'exp1_001_a.bmp' )
frame_b  = openpiv.tools.imread( 'exp1_001_b.bmp' )

u, v, sig2noise = openpiv.process.extended_search_area_piv( frame_a.astype(np.int32), frame_b.astype(np.int32), window_size=24, overlap=12, dt=0.02, search_area_size=64, sig2noise_method='peak2peak' )

x, y = openpiv.process.get_coordinates( image_size=frame_a.shape, window_size=24, overlap=12 )

u, v, mask = openpiv.validation.sig2noise_val( u, v, sig2noise, threshold = 1.3 )

u, v = openpiv.filters.replace_outliers( u, v, method='localmean', max_iter=10, kernel_size=2)

x, y, u, v = openpiv.scaling.uniform(x, y, u, v, scaling_factor = 96.52 )

openpiv.tools.save(x, y, u, v, mask, 'exp1_001.txt' )

openpiv.tools.display_vector_field('exp1_001.txt', scale=100, width=0.0025)
{% endhighlight %}

numpyがインストールされていて、frame_aを読み込むときにデータ型が指定されているのがわかります。
