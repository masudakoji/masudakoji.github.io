---
layout: post
title: "Jekyll Bootstrapでサイト構築"
description: ""
category: 
tags: [Ruby, Jekyll, Mac]
---
{% include JB/setup %}

## Jekyll Bootstrapとは

ウェブサイトを作る際、動的サイトならWordpress、静的サイトならSphinxとかhtmlでそのまま書くかして作るなどの方法がありますが、せっかくGitHub上でJekyllが使えるとのことなので、使ってみようと思った次第。本音を言うと慣れているSphinxの方がいいんだけど、Jekyllだと、Jekyll-Bootstrap（昔はTwitter-Bootstrapと言ったらしい？）のようなフレームワークもあるみたいなので、レスポンシブなサイトも簡単につくれるみたいです。

WordPressでサイトを構築したり、WordPressのテーマを作ったこともありますが、それはそれで結構めんどくさいですし笑、レスポンシブなサイトを作ろうとなるとなかなか手間もかかります。そもそも、一人で更新するんだから別にデータベースなんていらないじゃん、という事。

## インストール作業


### Jekyll-Bootstrapインストール

[公式サイト](http://jekyllbootstrap.com/usage/jekyll-quick-start.html)に書いてあるとおりにやれば大丈夫。以下は公式サイトからのコピペです。

	git clone https://github.com/plusjade/jekyll-bootstrap.git USERNAME.github.com
	cd USERNAME.github.com
	git remote set-url origin git@github.com:USERNAME/USERNAME.github.com.git
	git push origin master

ここで、cloneするときにローカルの名前が`USERNAME.github.com`になってますが、いまGitHub PageではページのURLは.comから.ioになってるので、`USERNAME.github.io`にしたほうが良いんじゃないかな。一応.comからもリダイレクトはされるそうですが、新しくGitHubでGitHub Pageを作るときには、リポジトリ名は`USERNAME.github.io`にしろ、といわれるはずです。なので上のコマンドでcloseする時やリモートリポジトリを登録するときには`.com`を`.io`に読み替えていただければ。

落としてきたJekyll-Bootstrapの設定ファイル`_config.yml`にはGitHub Pageのアドレスは`github.io`って書いてるのに、なんで公式サイトのコマンドでは反映されてないんだろう。

さて、当然のことながらgitでバージョン管理されるようになりますので、変更点がわかりやすくて良いのでは無いでしょうか。先程も述べましたが、GitHubにはJekyllは既にインストールされているので、Jekyll-Bootstrapディレクトリごとアップロードすれば良いです。またローカルでビルドして構築したサイトのデータ（_siteディレクトリ）を別のサーバにアップロードしても使えます。

また、`.gitignore`には

    _site/*

とだけ書き込んでおきましょう。出力ファイルをバージョン管理する必要はないので。


### ローカルにJekyll環境を
GitHubにはJekyllがインストールされているので、Jekyll-Bootstrapのディレクトリをそのままpushしても良いのですが、ローカルでもサイトがどのように表示されているか確認もしたいので、入れておいたら良いかと思います。JekyllはRubyで動いているので、Ruby環境の整備から。

### Ruby環境インストール
MacにはもともとRubyも入ってはいますが、もうすこし新しいバージョンの開発環境を作りましょう。Rubyのバージョンを切り替えるためにrbenvを使ってインストールします。rbenvはPythonでいうvirtualenvとかpyenv（rbenvの方が先らしいけど）のようなものです。

インストールは[Homebrew](http://brew.sh/)で行います。

    brew install rbenv ruby-build
    rbenv install 2.1.2
    rbenv rehash
    rbenv global 2.1.2


### Jekyllインストール

    gem update --system
    gem install jekyll

これでJekyllはインストールできました。ローカルでもサイトをビルドすることが出来ます。よく使うのは

    jekyll build

でビルドしたり

    jekyll serve

でローカルで表示のチェックしたりだと思います。