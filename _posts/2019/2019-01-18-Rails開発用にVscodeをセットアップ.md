---
layout: post
title: "Rails開発用にVscodeをセットアップ"
categories: vscode
---

## この記事で伝えること

- VSCodeのターミナルをMSYS2にする
- Ruby, Rails 開発用にインストールする拡張機能と設定

## ターミナルに MSYS2 を利用する

<https://qiita.com/yumetodo/items/42132a1e8435504448aa> このへんを参考に設定。
以下は自分の設定。

```json:setting.json
{
  "terminal.integrated.shell.windows": "E:\\Applications\\System\\msys64\\usr\\bin\\bash.exe",
  "terminal.integrated.env.windows": {
    "MSYSTEM": "MINGW64",
    "CHERE_INVOKING": "1",
    "MSYS2_PATH_TYPE": "inherit",
    "MSYS": "winsymlinks:nativestrict"
  },
  "terminal.integrated.shellArgs.windows": ["--login"],
  "terminal.integrated.cursorStyle": "line"
}
```

## dotfiles

msys2 のホームディレクトリは`msys2インストールディレクトリ/home/USERNAME` なので、Windows のユーザーディレクトリに置いてある必要な dotfiles はコピってくる必要あり。
ただし、間接的に利用されるものは不要のよう。具体的には.ssh はそのままユーザーディレクトリでも問題がない。
.gemrc はコピってくる必要があった。この辺の要不要判断は使いながらかな。。。

## Ruby 用拡張機能

### 今入れてるもの

チュートリアル進むにつれ増える可能性あり。

- Ruby
- Ruby Solargraph
- Beautify
- ruby-symbols
- endwise

### 必要 GEM

拡張機能には gem を利用しているものがあるので、gem をグローバルにインストールします。
拡張機能の説明に書いてあります。

- rubocop
- solargraph
- ruby-debug-ide
- debase (or byebug)

### 設定

各拡張機能の説明を読んだほうがいいと思います。
パスは読み替えてください。

```json:setting.json
{
    // solargraph
    "solargraph.diagnostics": true,
    "solargraph.commandPath": "E:\\Applications\\System\\Ruby\\ruby2431\\bin\\solargraph.bat",
    "solargraph.autoformat": true,
    "solargraph.symbols": true,
    "solargraph.completion": true,

    // ruby
    "ruby.format": false,
    "ruby.intellisense": false,
    "ruby.lint": {
        "reek": true,
        "rubocop": false,
        "ruby": true, //Runs ruby -wc
        "fasterer": true,
        "debride": true,
        "ruby-lint": true
    },
    "ruby.codeCompletion": false,
    "ruby.useLanguageServer": false,
    "ruby.locate": {
        "exclude": "{**/@(test|spec|tmp|.*),**/@(test|spec|tmp|.*)/**,**/*_spec.rb}",
        "include": "**/*.rb"
    },

    // beautify(erbをhtmlに追加)
    "beautify.language": {
        "js": {
          "type": ["javascript", "json"],
          "filename": [".jshintrc", ".jsbeautifyrc"]
          // "ext": ["js", "json"]
          // ^^ to set extensions to be beautified using the javascript beautifier
        },
        "css": ["css", "scss"],
        "html": ["htm", "html", "erb"],
    }
    // ^^ providing just an array sets the VS Code file type
}
```
