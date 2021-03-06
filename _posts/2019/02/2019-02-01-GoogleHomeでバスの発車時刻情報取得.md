---
layout: post
title: "GoogleHomeでバスの発車時刻情報取得"
categories: GoogleHome ActionsOnGoogle DialogFlow Python Django
---

バスって全く時間通りに来ないですね。

嫁が「朝バス来なくて(；´Д｀)」みたいなことをちょいちょい言っているのを思い出し、
Web上のバスのリアルタイム位置情報を取得してGoogleHomeにしゃべらせてみることに。

なにで作ってもよかったけどスクレイピングといえばPythonみたいな勝手な思い込みから、Pythonで作ることに。
DialogFlowからAPI呼び出してそのレスポンスを使ってしゃべらせることができるようだったので、 Python,WebAPI で調べてDjangoに行き着く。
レンタルサーバではWSGIではなくCGIから動かすしかないようで、天才が作ったCGIからDjangoアプリケーションを動かすモジュールを見つけてきて構築。

～～
関係ないけど最近はサーバ側にGitのリポジトリを立ててプッシュすることでサイトを更新するテクが主流らしく。
超便利じゃんと思いながら設定したら超便利だった話はいずれするかもしれない
～～

ということでこんな構成に。  
<https://github.com/h-okhs/seibubus/blob/master/%E6%A7%8B%E6%88%90.pdf>

理想的にはAPIはバスの情報をJSONで返して、DialogFlow側でスピーチ内容に変換できればよかったけど、
インラインのフルフィルメントエディターでのAPI呼び出し⇒JSONがうまくできなかった。ログが出せないので何がダメかわからず仕舞。
仕方なくAPIから直接スピーチ内容を含んだ所定の形式のJSONを返す形で実装。

丸二日かかってとりあえずしゃべるようにはなりました。
GoogleHome(アシスタント)からアプリを起動するっていうスタイルじゃなく、シームレスにアプリと通信できればいいんだけどな。