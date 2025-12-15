# Leaked Flag Checker

> みんなだいすきフラグチェッカー

[leaked-flag-checker.ar.gz](https://alpacahack-prod.s3.ap-northeast-1.amazonaws.com/0bbdd0f6-3b81-4c67-9b16-1febd6641ec9/leaked-flag-checker.tar.gz)が提供される

```
> ls
challenge*  challenge.c
```

とりあえずコード見る

```c
// gcc -o challenge challenge.c
#include <stdio.h>
#include <string.h>

int main(void) {
    char input[32];
    const char xor_flag[] = "REDACTED";
    size_t flag_len = strlen(xor_flag);

    printf("Enter flag: ");
    fflush(stdout);
    scanf("%31s", input);

    if(strlen(input) != flag_len) {
        printf("Wrong length\n");
        return 1;
    }
    for(size_t i = 0; i < flag_len; i++) {
        if((input[i] ^ 7) != xor_flag[i]) {
            printf("Wrong at index %zu\n", i);
            return 1;
        }
    }
    printf("Correct\n");
    return 0;
}
```

xor_flagを7でXORすればいい。

r2を使う。

```bash
[0x72049db4c440]> pdf @ main
            ; DATA XREF from entry0 @ 0x6388f6f89118(r)
┌ 303: int main (int argc, char **argv, char **envp);
│ afv: vars(6:sp[0x10..0x58])
│           0x6388f6f891e9      f30f1efa       endbr64
│           0x6388f6f891ed      55             push rbp
│           0x6388f6f891ee      4889e5         mov rbp, rsp
│           0x6388f6f891f1      4883ec50       sub rsp, 0x50
│           0x6388f6f891f5      64488b0425..   mov rax, qword fs:[0x28]
│           0x6388f6f891fe      488945f8       mov qword [var_8h], rax
│           0x6388f6f89202      31c0           xor eax, eax
│           0x6388f6f89204      48b8466b77..   movabs rax, 0x6b7c666466776b46 ; 'Fkwfdf|k'
│           0x6388f6f8920e      488945c2       mov qword [var_3eh], rax
│           0x6388f6f89212      48b87c6b72..   movabs rax, 0x7a7e6c64726b7c ; '|krdl~z'
│           0x6388f6f8921c      488945c8       mov qword [var_38h], rax
│           0x6388f6f89220      48c745b80d..   mov qword [var_48h], 0xd ; 13
│           0x6388f6f89228      488d05d50d..   lea rax, str.Enter_flag: ; 0x6388f6f8a004 ; "Enter flag: "
│           0x6388f6f8922f      4889c7         mov rdi, rax
```

あきらかに`Fkwfdf|k`と`|krdl~z`があやしい

```py
for s in [b"Fkwfdf|kH", b"|krdl~z"]:
    print(bytes(c^7 for c in s))
```

```
b'Alpaca{lO'
b'{lucky}'
```

`Alpaca{lO{lucky}`に見えるが、違う。

`{`と`l`と`O`は偶然printableにみえるだけ。

試してみる。

```
> ./challenge
Enter flag: Alpaca{lO{lucky}
Wrong length
```

```
> ./challenge
Enter flag: Alpaca{lucky}
Correct
```

flag: `Alpaca{lucky}`
