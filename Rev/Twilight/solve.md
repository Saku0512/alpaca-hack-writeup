# Twilight

> 黄昏時に...

[twilight.tar.gz](https://alpacahack-prod.s3.ap-northeast-1.amazonaws.com/efed6c96-b622-4eaf-8e9a-44f1f733f4ae/twilight.tar.gz)が提供される。

解凍する
```
twilight/
twilight/chall
twilight/out.txt
```

out.txt
```txt
0x41, 0x6D, 0x72, 0x62, 0x67, 0x64, 0x81, 0x46, 0x74, 0x79, 0x6B, 0x68, 0x6D, 0x45, 0x6F, 0x6C, 0x7B, 0x4E, 0x7B, 0x7D, 0x73, 0x42, 0x85, 0x79, 0x7C, 0x7C, 0x8C, 0x77, 0x7D, 0x73, 0x82, 0x62,
```

r2でmain関数を見る
```bash
            ; ICOD XREF from entry0 @ 0x10f8(r)
┌ 239: int main (int argc, char **argv, char **envp);
│ afv: vars(6:sp[0x10..0x5c])
│           0x000011f7      f30f1efa       endbr64
│           0x000011fb      55             push rbp
│           0x000011fc      4889e5         mov rbp, rsp
│           0x000011ff      53             push rbx
│           0x00001200      4883ec58       sub rsp, 0x58
│           0x00001204      64488b0425..   mov rax, qword fs:[0x28]
│           0x0000120d      488945e8       mov qword [canary], rax
│           0x00001211      31c0           xor eax, eax
│           0x00001213      488d05ea0d..   lea rax, str.Enter_the_flag: ; 0x2004 ; "Enter the flag: "
│           0x0000121a      4889c7         mov rdi, rax                ; const char *format
│           0x0000121d      b800000000     mov eax, 0
│           0x00001222      e899feffff     call sym.imp.printf         ; int printf(const char *format)
│           0x00001227      488d45c0       lea rax, [s]
│           0x0000122b      4889c6         mov rsi, rax
│           0x0000122e      488d05e00d..   lea rax, str._33s           ; 0x2015 ; "%33s"
│           0x00001235      4889c7         mov rdi, rax                ; const char *format
│           0x00001238      b800000000     mov eax, 0
│           0x0000123d      e88efeffff     call sym.imp.__isoc99_scanf ; int scanf(const char *format)
│           0x00001242      488d0580ff..   lea rax, [sym.a]            ; 0x11c9
│           0x00001249      488945b0       mov qword [var_50h], rax
│           0x0000124d      488d058dff..   lea rax, [sym.b]            ; 0x11e1
│           0x00001254      488945b8       mov qword [var_48h], rax
│           0x00001258      c745ac0000..   mov dword [var_54h], 0
│       ┌─< 0x0000125f      eb45           jmp 0x12a6
│       │   ; CODE XREF from main @ 0x12bb(x)
│      ┌──> 0x00001261      8b45ac         mov eax, dword [var_54h]
│      ╎│   0x00001264      99             cdq
│      ╎│   0x00001265      c1ea1f         shr edx, 0x1f
│      ╎│   0x00001268      01d0           add eax, edx
│      ╎│   0x0000126a      83e001         and eax, 1
│      ╎│   0x0000126d      29d0           sub eax, edx
│      ╎│   0x0000126f      4898           cdqe
│      ╎│   0x00001271      488b4cc5b0     mov rcx, qword [rbp + rax*8 - 0x50]
│      ╎│   0x00001276      8b55ac         mov edx, dword [var_54h]
│      ╎│   0x00001279      8b45ac         mov eax, dword [var_54h]
│      ╎│   0x0000127c      4898           cdqe
│      ╎│   0x0000127e      0fb64405c0     movzx eax, byte [rbp + rax - 0x40]
│      ╎│   0x00001283      0fb6c0         movzx eax, al
│      ╎│   0x00001286      89d6           mov esi, edx
│      ╎│   0x00001288      89c7           mov edi, eax
│      ╎│   0x0000128a      ffd1           call rcx
│      ╎│   0x0000128c      89c6           mov esi, eax
│      ╎│   0x0000128e      488d05850d..   lea rax, str.0x_X_          ; str.0x_X_
│      ╎│                                                              ; 0x201a ; "0x%X, "
│      ╎│   0x00001295      4889c7         mov rdi, rax                ; const char *format
│      ╎│   0x00001298      b800000000     mov eax, 0
│      ╎│   0x0000129d      e81efeffff     call sym.imp.printf         ; int printf(const char *format)
│      ╎│   0x000012a2      8345ac01       add dword [var_54h], 1
│      ╎│   ; CODE XREF from main @ 0x125f(x)
│      ╎└─> 0x000012a6      8b45ac         mov eax, dword [var_54h]
│      ╎    0x000012a9      4863d8         movsxd rbx, eax
│      ╎    0x000012ac      488d45c0       lea rax, [s]
│      ╎    0x000012b0      4889c7         mov rdi, rax                ; const char *s
│      ╎    0x000012b3      e8e8fdffff     call sym.imp.strlen         ; size_t strlen(const char *s)
│      ╎    0x000012b8      4839c3         cmp rbx, rax
│      └──< 0x000012bb      72a4           jb 0x1261
│           0x000012bd      bf0a000000     mov edi, 0xa                ; int c
│           0x000012c2      e8c9fdffff     call sym.imp.putchar        ; int putchar(int c)
│           0x000012c7      b800000000     mov eax, 0
│           0x000012cc      488b55e8       mov rdx, qword [canary]
│           0x000012d0      64482b1425..   sub rdx, qword fs:[0x28]
│       ┌─< 0x000012d9      7405           je 0x12e0
│       │   0x000012db      e8d0fdffff     call sym.imp.__stack_chk_fail ; void __stack_chk_fail(void)
│       │   ; CODE XREF from main @ 0x12d9(x)
│       └─> 0x000012e0      488b5df8       mov rbx, qword [var_8h]
│           0x000012e4      c9             leave
└           0x000012e5      c3             ret
```

sym.aとsym.bが怪しい
```bash
[0x000010e0]> pdf @ sym.a
            ; ICOD XREF from main @ 0x1242(r)
┌ 24: sym.a (int64_t arg1, int64_t arg2);
│ `- args(rdi, rsi) vars(2:sp[0xc..0x10])
│           0x000011c9      f30f1efa       endbr64
│           0x000011cd      55             push rbp
│           0x000011ce      4889e5         mov rbp, rsp
│           0x000011d1      897dfc         mov dword [var_4h], edi     ; arg1
│           0x000011d4      8975f8         mov dword [var_8h], esi     ; arg2
│           0x000011d7      8b55fc         mov edx, dword [var_4h]
│           0x000011da      8b45f8         mov eax, dword [var_8h]
│           0x000011dd      01d0           add eax, edx
│           0x000011df      5d             pop rbp
└           0x000011e0      c3             ret
[0x000010e0]> pdf @ sym.b
            ; ICOD XREF from main @ 0x124d(r)
┌ 22: sym.b (int64_t arg1, int64_t arg2);
│ `- args(rdi, rsi) vars(2:sp[0xc..0x10])
│           0x000011e1      f30f1efa       endbr64
│           0x000011e5      55             push rbp
│           0x000011e6      4889e5         mov rbp, rsp
│           0x000011e9      897dfc         mov dword [var_4h], edi     ; arg1
│           0x000011ec      8975f8         mov dword [var_8h], esi     ; arg2
│           0x000011ef      8b45fc         mov eax, dword [var_4h]
│           0x000011f2      3345f8         xor eax, dword [var_8h]
│           0x000011f5      5d             pop rbp
└           0x000011f6      c3             ret
```

文字の長さ分だけループしている。
カウンタiが奇数が偶数で処理を分けている。

- 偶数
	- result = c + i
- 奇数
	- result = c ^ i

decodeする。
```py
# out.txtの中身
enc_data = [
    0x41, 0x6D, 0x72, 0x62, 0x67, 0x64, 0x81, 0x46,
    0x74, 0x79, 0x6B, 0x68, 0x6D, 0x45, 0x6F, 0x6C,
    0x7B, 0x4E, 0x7B, 0x7D, 0x73, 0x42, 0x85, 0x79,
    0x7C, 0x7C, 0x8C, 0x77, 0x7D, 0x73, 0x82, 0x62
]

flag = ""

for i, val in enumerate(enc_data):
    if i % 2 == 0:
        # 偶数インデックス (sym.a): val = char + i
        # 逆算: char = val - i
        char_code = val - i
    else:
        # 奇数インデックス (sym.b): val = char ^ i
        # 逆算: char = val ^ i
        char_code = val ^ i

    flag += chr(char_code)

print(f"FLAG: {flag}")
```

```bash
> python decode.py
FLAG: Alpaca{AlpacaHack_in_Wonderland}
```

flag: `Alpaca{AlpacaHack_in_Wonderland}`
