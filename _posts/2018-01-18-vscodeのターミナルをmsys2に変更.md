<https://qiita.com/yumetodo/items/42132a1e8435504448aa> このへん見て設定。
Windowsの環境変数が引き継がれなかったのでenvに`"MSYS2_PATH_TYPE": "inherit"`を追加してみた。動いてるけどこれで良いのか悪いのかは不明。

### 2018-01-23追記
msys2のホームディレクトリは`msys2インストールディレクトリ/home/USERNAME` なので、Windowsのユーザーディレクトリに置いてある必要なdotfilesはコピってくる必要あり。

シンボリックリンクを使用したい場合はenvに`"MSYS": "winsymlinks:nativestrict"`を追加する。
