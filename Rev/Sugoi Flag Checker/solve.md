# Sugoi Flag Checker

> すごい暗号を使ったフラグチェッカー

[sugoi-flag-checker.tar.gz](https://alpacahack-prod.s3.ap-northeast-1.amazonaws.com/6e83a132-e576-493d-afb4-00f2129b62b8/sugoi-flag-checker.tar.gz)が提供される

r2で解析する

とりあえずmainみる

```bash
[0x74c9d2519440]> pdf @ main
            ; DATA XREF from entry0 @ 0x5cf65a334118(r)
┌ 389: int main (int argc, char **argv, char **envp);
│ afv: vars(10:sp[0x10..0x188])
│           0x5cf65a334feb      f30f1efa       endbr64
│           0x5cf65a334fef      55             push rbp
│           0x5cf65a334ff0      4889e5         mov rbp, rsp
│           0x5cf65a334ff3      4881ec8001..   sub rsp, 0x180
│           0x5cf65a334ffa      64488b0425..   mov rax, qword fs:[0x28]
│           0x5cf65a335003      488945f8       mov qword [var_8h], rax
│           0x5cf65a335007      31c0           xor eax, eax
│           0x5cf65a335009      48c78580fe..   mov qword [var_180h], 0
│           0x5cf65a335014      488d054f12..   lea rax, str.flag:      ; 0x5cf65a33626a ; "flag: "
│           0x5cf65a33501b      4889c7         mov rdi, rax
│           0x5cf65a33501e      b800000000     mov eax, 0
│           0x5cf65a335023      e898f0ffff     call sym.imp.printf     ; int printf(const char *format)
│           0x5cf65a335028      488d9580fe..   lea rdx, [var_180h]
│           0x5cf65a33502f      488d8570ff..   lea rax, [var_90h]
│           0x5cf65a335036      be80000000     mov esi, 0x80           ; 128
│           0x5cf65a33503b      4889c7         mov rdi, rax
│           0x5cf65a33503e      e82fffffff     call sym.read_flag_input
│           0x5cf65a335043      83f001         xor eax, 1
│           0x5cf65a335046      84c0           test al, al
│       ┌─< 0x5cf65a335048      740a           je 0x5cf65a335054
│       │   0x5cf65a33504a      b801000000     mov eax, 1
│      ┌──< 0x5cf65a33504f      e906010000     jmp 0x5cf65a33515a
│      │└─> 0x5cf65a335054      488b8580fe..   mov rax, qword [var_180h]
│      │    0x5cf65a33505b      ba20000000     mov edx, 0x20           ; 32
│      │    0x5cf65a335060      4839d0         cmp rax, rdx
│      │┌─< 0x5cf65a335063      7419           je 0x5cf65a33507e
│      ││   0x5cf65a335065      488d050512..   lea rax, str.wrong...   ; 0x5cf65a336271 ; "wrong..."
│      ││   0x5cf65a33506c      4889c7         mov rdi, rax
│      ││   0x5cf65a33506f      e82cf0ffff     call sym.imp.puts       ; int puts(const char *s)
│      ││   0x5cf65a335074      b800000000     mov eax, 0
│     ┌───< 0x5cf65a335079      e9dc000000     jmp 0x5cf65a33515a
│     ││└─> 0x5cf65a33507e      488b05bb11..   mov rax, qword [obj.ciphertext] ; [0x5cf65a336240:8]=0x8b1082b46c103dae
│     ││    0x5cf65a335085      488b15bc11..   mov rdx, qword [0x5cf65a336248] ; [0x5cf65a336248:8]=0xb0a297ae01c97a9f
│     ││    0x5cf65a33508c      48898540ff..   mov qword [var_c0h], rax
│     ││    0x5cf65a335093      48899548ff..   mov qword [var_b8h], rdx
│     ││    0x5cf65a33509a      488b05af11..   mov rax, qword [0x5cf65a336250] ; [0x5cf65a336250:8]=0x3c422bd6a54a2f40
│     ││    0x5cf65a3350a1      488b15b011..   mov rdx, qword [0x5cf65a336258] ; [0x5cf65a336258:8]=0x33838314a4bf7ecc
│     ││    0x5cf65a3350a8      48898550ff..   mov qword [var_b0h], rax
│     ││    0x5cf65a3350af      48899558ff..   mov qword [var_a8h], rdx
│     ││    0x5cf65a3350b6      488d8590fe..   lea rax, [var_170h]
│     ││    0x5cf65a3350bd      488d156c11..   lea rdx, obj.key        ; 0x5cf65a336230
│     ││    0x5cf65a3350c4      4889d6         mov rsi, rdx
│     ││    0x5cf65a3350c7      4889c7         mov rdi, rax
│     ││    0x5cf65a3350ca      e84ffeffff     call sym.init_cipher
│     ││    0x5cf65a3350cf      48c78588fe..   mov qword [var_178h], 0
│     ││┌─< 0x5cf65a3350da      eb2b           jmp 0x5cf65a335107
│    ┌────> 0x5cf65a3350dc      488d9540ff..   lea rdx, [var_c0h]
│    ╎│││   0x5cf65a3350e3      488b8588fe..   mov rax, qword [var_178h]
│    ╎│││   0x5cf65a3350ea      4801c2         add rdx, rax
│    ╎│││   0x5cf65a3350ed      488d8590fe..   lea rax, [var_170h]
│    ╎│││   0x5cf65a3350f4      4889d6         mov rsi, rdx
│    ╎│││   0x5cf65a3350f7      4889c7         mov rdi, rax
│    ╎│││   0x5cf65a3350fa      e849feffff     call sym.decrypt_block
│    ╎│││   0x5cf65a3350ff      48838588fe..   add qword [var_178h], 0x10 ; [0x10:8]=-1 ; 16
│    ╎│││   ; CODE XREF from main @ 0x5cf65a3350da(x)
│    ╎││└─> 0x5cf65a335107      4883bd88fe..   cmp qword [var_178h], 0x1f
│    └────< 0x5cf65a33510f      76cb           jbe 0x5cf65a3350dc
│     ││    0x5cf65a335111      c68560ffff..   mov byte [var_a0h], 0
│     ││    0x5cf65a335118      488d9540ff..   lea rdx, [var_c0h]
│     ││    0x5cf65a33511f      488d8570ff..   lea rax, [var_90h]
│     ││    0x5cf65a335126      4889d6         mov rsi, rdx
│     ││    0x5cf65a335129      4889c7         mov rdi, rax
│     ││    0x5cf65a33512c      e8bfefffff     call sym.imp.strcmp     ; int strcmp(const char *s1, const char *s2)
│     ││    0x5cf65a335131      85c0           test eax, eax
│     ││┌─< 0x5cf65a335133      7511           jne 0x5cf65a335146
│     │││   0x5cf65a335135      488d053e11..   lea rax, str.correct_   ; 0x5cf65a33627a ; "correct!"
│     │││   0x5cf65a33513c      4889c7         mov rdi, rax
│     │││   0x5cf65a33513f      e85cefffff     call sym.imp.puts       ; int puts(const char *s)
│    ┌────< 0x5cf65a335144      eb0f           jmp 0x5cf65a335155
│    │││└─> 0x5cf65a335146      488d052411..   lea rax, str.wrong...   ; 0x5cf65a336271 ; "wrong..."
│    │││    0x5cf65a33514d      4889c7         mov rdi, rax
│    │││    0x5cf65a335150      e84befffff     call sym.imp.puts       ; int puts(const char *s)
│    │││    ; CODE XREF from main @ 0x5cf65a335144(x)
│    └────> 0x5cf65a335155      b800000000     mov eax, 0
│     ││    ; CODE XREFS from main @ 0x5cf65a33504f(x), 0x5cf65a335079(x)
│     └└──> 0x5cf65a33515a      488b55f8       mov rdx, qword [var_8h]
│           0x5cf65a33515e      64482b1425..   sub rdx, qword fs:[0x28]
│       ┌─< 0x5cf65a335167      7405           je 0x5cf65a33516e
│       │   0x5cf65a335169      e842efffff     call sym.imp.__stack_chk_fail ; void __stack_chk_fail(void)
│       └─> 0x5cf65a33516e      c9             leave
└           0x5cf65a33516f      c3             ret
[0x74c9d2519440]>
```

入力したflagが32文字か確かめていろいろ暗号とかやってる。
`afl`でみた関数一覧からおそらくAESであることが分かる。

しかし、`strcmp`する直前の文字はflagの平文になっているはずなのでそこを求める。

`ltrace`を使う。

flagは適当に`11111111111111111111111111111111`を入力する

```bash
> ltrace ./chal
printf("flag: ")                                                                                                                 = 6
fgets(flag: 11111111111111111111111111111111
"11111111111111111111111111111111"..., 128, 0x7dfd0090b8e0)                                                                = 0x7ffd5c2140f0
strcspn("11111111111111111111111111111111"..., "\n")                                                                             = 32
strcmp("11111111111111111111111111111111"..., "Alpaca{m3ccha_rand0m_5b0x_d4yo!}"...)                                             = -16
puts("wrong..."wrong...
)                                                                                                                 = 9
+++ exited (status 0) +++
```

flag: `Alpaca{m3ccha_rand0m_5b0x_d4yo!}`
