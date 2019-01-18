# Ruby開発環境構築

色々やり始めると変わってくる部分もあると思われますが、ひとまずチュートリアルが作れる環境を構築。

## Ruby本体

Rubyのバージョンは切り替えられるようにしておいたほうがよさそう(2.5系ではチュートリアルが動かずいきなりつまずいたという・・・)。
chocolatry(パッケージ管理), uru(rbenvみたいなの)といったツールを利用してruby本体などのインストールを行う。

1. 前準備
   1. gemインストール時のデフォルトオプションの指定：%USERPROFILE%\.gemrc に gem: --no-ri --no-rdoc
2. chocolateyインストール
   1. <https://chocolatey.org/install>
   2. 環境変数変更 ChocolateyToolsLocation(chocolateyでのパッケージインストール先)
3. rubyインストール
   1. $ cinst ruby –version 2.4.3.1 -ia ‘/dir=C:\tools\ruby2431’
   2. 二つ目以降は-forceをつける
4. uruインストール(Ruby2.6からridk useで変えられるので不要になるらしい?)
   1. <https://bitbucket.org/jonforums/uru/wiki/Downloads>
   2. $ cinst uru.0.8.5.nupkg
   3. $ Set-ExecutionPolicy -ExecutionPolicy RemoteSigned
   4. $ uru admin add E:\tools\ruby2431\bin --tag ruby243
5. MSYS2インストール
   1. $ cinst MSYS2 --params "/NoUpdate"
   2. cmd/powershell再起動(VSCodeのターミナルの場合はVSCode再起動)
   3. $ ridk install 2 3
6. 各種GEMインストール
   1. $ uru ruby243
   2. $ gem install bunder

## gemについて

gemのインストールには二種類あるそう。PC内のRuby環境全体に適用されるものと、アプリケーション内にのみ適用されるもの。よく見るgemコマンドでインストールした場合は、Ruby環境全体に適用される。ただし、全体に適用してしまうとバージョンの整合性を取るのが難しくなるため(？)、あまり使わないのが一般的のよう。
代わりにBundlerというgemを利用してアプリケーションごとにgemをインストールするのが良いらしい。
ということでBundlerを使ってRailsアプリケーションを作っていく。

## Railsアプリケーション生成

railsアプリケーションを作成するためにrailsを使いたいので、bundlerでまずrailsのみをインストールしてアプリケーションを生成する。その後アプリケーション内のGemfileを使用して各種gemをアプリケーション内にのみインストールする。
以下はE:\repo\rails01に生成する場合。

1. $ uru ruby243
2. E:\repoで $ bundle init
3. 生成されたGemfile変更 gem "rails", "5.1.6"
4. $ bundle install --path=vendor/bundle
5. $ bundle exec rails new rails01 --skip-bundle(ruby本体にインストールされるのを防止) -T(テスト生成しない)
6. ※オプション .bundle, vendor, gemfile, gemfile.lockを削除
7. $ cd rails01
8. $ bundle install --path=vendor/bundle
9. gitignoreに/vendor追加
