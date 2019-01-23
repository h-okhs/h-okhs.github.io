---
layout: post
title:  "Ruby開発環境構築(Windows)"
categories: Ruby
---

色々やり始めると変わってくる部分もあると思われますが、ひとまずRailsチュートリアルを進められる環境を構築。

## この記事で伝えること

- Rubyのインストール
- 複数バージョンのRubyへの対応(使用するRubyのバージョンが切り替えられる)
- Railsアプリケーション雛形の作成
- Gemインストール領域の理解(グローバル or アプリケーション)

エディタはVSCodeを使用しますが、そちらについては別途書きます。

## Ruby本体

Rubyのバージョンは切り替えられるようにしておいたほうがよさそう(2.5系ではチュートリアルが動かずいきなりつまずいたという・・・)。
chocolatry(パッケージ管理), uru(rbenvみたいなの)といったツールを利用してruby本体などのインストールを行う。
Rubyチュートリアルは2.5系以降では動かないようなので2.4.3を入れます。

1. 前準備
   1. gemインストール時のデフォルトオプションの指定：`%USERPROFILE%\.gemrc` に `gem: --no-ri --no-rdoc`
2. chocolateyインストール
   1. <https://chocolatey.org/install>
   2. 環境変数変更 ChocolateyToolsLocation(chocolateyでのパッケージインストール先)
3. rubyインストール
   1. `$ cinst ruby –version 2.4.3.1 -ia ‘/dir=C:\tools\ruby2431’`
   2. 二つ目以降は-forceをつける
4. uruインストール(Ruby2.6からridk useで変えられるので不要になるらしい?)
   1. <https://bitbucket.org/jonforums/uru/wiki/Downloads>
   2. `$ cinst uru.0.8.5.nupkg`
   3. `$ Set-ExecutionPolicy -ExecutionPolicy RemoteSigned`
   4. `$ uru admin add E:\tools\ruby2431\bin --tag ruby243`
5. MSYS2インストール
   1. `$ cinst MSYS2 --params "/NoUpdate"`
   2. 前準備で作った.gemrcを`msys2/home/USERNAME`にコピー
   3. cmd/powershell再起動(VSCodeのターミナルの場合はVSCode再起動)
   4. `$ ridk install 2 3`
6. 各種GEMインストール
   1. `$ uru ruby243`
   2. `$ gem install bunder`

## gemについて

gemのインストールには二種類あるそう。PC内のRuby環境全体に適用されるものと、アプリケーション内にのみ適用されるもの。よく見るgemコマンドでインストールした場合は、Ruby環境全体に適用される。ただし、全体に適用してしまうとバージョンの整合性を取るのが難しくなるため(？)、あまり使わないのが一般的のよう。
代わりにBundlerというgemを利用してアプリケーションごとにgemをインストールするのが良いらしい。
ということでBundlerを使ってRailsアプリケーションを作っていく。

## Railsアプリケーション生成

railsアプリケーションを作成するためにrailsを使いたいので、bundlerでまずrailsのみをインストールしてアプリケーションを生成する。その後アプリケーション内のGemfileを使用して各種gemをアプリケーション内にのみインストールする。
以下はE:\repo\rails01に生成する場合。

1. `$ uru ruby243`
2. E:\repoで `$ bundle init`
3. 生成されたGemfile変更 `gem "rails", "5.1.6"`
4. `$ bundle install --path=vendor/bundle`
5. `$ bundle exec rails new rails01 --skip-bundle(グローバルにインストールされるのを防止) -T(テスト生成しない)`
6. ※オプション .bundle, vendor, gemfile, gemfile.lockを削除
7. `$ cd rails01`
8. `$ bundle install --path=vendor/bundle --without production`
9. gitignoreに/vendor追加
10. 生成されたGemfileのjbuilderの下あたりに`gem 'coffee-script-source', '1.8.0'`追加 (これがないとエラーが出る)
11. `$ bundle install`

手順8でbundle install時に`--path=vendor/bundle`をつけていることで、インストールされるgemは`rails01/vendor/bundle`内にインストールされることになり、rails01内だけで適用される形になる。
インストールしたgemを実行する場合は、`$ bundle exec rails console`などのように`bundle exec`をつけて実行することで`vendor/bundle`内のgemを使用して実行できる。
`bundle install`実行時のオプションは`.bundle/config`に保存され、次回以降の`bundle install`に自動的に適用されるため、手順10,11のようにGemfileにgemを追加インストール場合など、2回目以降は`bundle install`のみでOK。
