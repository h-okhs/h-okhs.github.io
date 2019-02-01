---
layout: post
title: "Rails開発用にVscodeをセットアップ"
categories: vscode
---

## この記事でやろうとしていること

- VSCodeのターミナルをMSYS2にする
- Ruby, Rails開発用の拡張機能インストールと設定

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
- rufo
- Ruby Comment Doc
- htmlbeautifier
- ruby-symbols
- endwise

### 必要 GEM

拡張機能には gem を利用しているものがあるので、gem をグローバルにインストールします。
拡張機能の説明に書いてあります。

- rubocop
- rcodetools
- fasterer
- reek
- ruby-debug-ide
- debase
- htmlbeautifier
- debride
- rubyLocate

### 設定

各拡張機能の説明を読んだほうがいいと思います。

```json:setting.json
{
  "ruby.format": false,
  "ruby.codeCompletion": "rcodetools",
  "ruby.intellisense": false,
  "ruby.lint": {
    "rubocop": {
      "lint": true, //enable all lint cops.
      // "only": [ /* array: Run only the specified cop(s) and/or cops in the specified departments. */ ],
      // "except": [ /* array: Run all cops enabled by configuration except the specified cop(s) and/or departments. */ ],
      // "forceExclusion": true, //Add --force-exclusion option
      // "require": [ /* array: Require Ruby files. */ ],
      "rails": true //Run extra rails cops
    },
    "ruby": true, //Runs ruby -wc
    "reek": true,
    "fasterer": true,
    "ruby-lint": true,
    "debride": {
      "rails": true //Add some rails call conversions.
    },
  },
  "ruby.useLanguageServer": true,
  "ruby.locate": {
    "exclude": "{**/@(test|spec|tmp|.*),**/@(test|spec|tmp|.*)/**,**/*_spec.rb}",
    "include": "**/*.rb"
  },
}
```
