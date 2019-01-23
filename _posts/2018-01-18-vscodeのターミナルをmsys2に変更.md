---
layout: post
title:  "日記 2019/01/23"
categories: diary
---

<https://qiita.com/yumetodo/items/42132a1e8435504448aa> このへん見て設定。
Windowsの環境変数が引き継がれなかったので`"terminal.integrated.env.windows"`に`"MSYS2_PATH_TYPE": "inherit"`を追加してみた。動いてるけどこれで良いのか悪いのかは不明。

### 2018-01-23追記
msys2のホームディレクトリは`msys2インストールディレクトリ/home/USERNAME` なので、Windowsのユーザーディレクトリに置いてある必要なdotfilesはコピってくる必要あり。

dotfilesをgitに登録することにした関係で、`msys2/home/USERNAME`からシンボリックリンクを張ることに。
msys2でシンボリックリンクを使用したい場合は、`"terminal.integrated.env.windows"`に`"MSYS": "winsymlinks:nativestrict"`を追加する。
(msys2インストール時にシンボリックリンク使うみたいな設定をONにしとけばいらないかも？)
