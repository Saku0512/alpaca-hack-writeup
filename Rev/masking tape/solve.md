# masking tape

> マスキングテープはフラグチェックにも便利です :)

`masking-tape`というファイルが提供される

```bash
> file masking-tape
masking-tape: ELF 64-bit LSB pie executable, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, BuildID[sha1]=61a9c2ab285b526528b77f1170889a951fb7efe2, for GNU/Linux 3.2.0, not stripped
```

実行可能ファイルであると分かる。

実行してみる。

```bash
> ./masking-tape
usage: ./masking-tape <input>

> ./masking-tape hoge
wrong
```

おそらく正しいflagをinputに入れるとCorrenctとか出ると思う。

r2で解析する。

関数一覧(一部抜粋)
```bash
0x601225b67090    1     11 sym.imp.puts
0x601225b670a0    1     11 sym.imp.strlen
0x601225b670b0    1     11 sym.imp.printf
0x601225b670c0    1     11 sym.imp.strcmp
0x601225b670d0    1     11 sym.imp.malloc
0x601225b670e0    1     38 entry0
0x601225b69fd8    1   4137 fcn.601225b69fd8
0x601225b67110    4     34 sym.deregister_tm_clones
0x601225b67140    4     51 sym.register_tm_clones
0x601225b67180    5     54 entry.fini0
0x601225b67080    1     11 fcn.601225b67080
0x601225b671c0    1      9 entry.init0
0x601225b673a4    1     13 sym._fini
0x601225b671c9   14    472 main
0x601225b67000    3     27 sym._init
0x7532903c987b    3     40 fcn.7532903c987b
```

とりあえず`main`を見る

```bash
            ; DATA XREF from entry0 @ 0x601225b670f8(r)
┌ 472: int main (int argc, char **argv);
│ `- args(rdi, rsi) vars(7:sp[0x10..0x48])
│           0x601225b671c9      f30f1efa       endbr64
│           0x601225b671cd      55             push rbp
│           0x601225b671ce      4889e5         mov rbp, rsp
│           0x601225b671d1      4883ec40       sub rsp, 0x40
│           0x601225b671d5      897dcc         mov dword [var_34h], edi ; argc
│           0x601225b671d8      488975c0       mov qword [var_40h], rsi ; argv
│           0x601225b671dc      837dcc02       cmp dword [var_34h], 2
│       ┌─< 0x601225b671e0      7428           je 0x601225b6720a
│       │   0x601225b671e2      488b45c0       mov rax, qword [var_40h]
│       │   0x601225b671e6      488b00         mov rax, qword [rax]
│       │   0x601225b671e9      4889c6         mov rsi, rax
│       │   0x601225b671ec      488d056a0e..   lea rax, str.usage:__s__input__n ; 0x601225b6805d ; "usage: %s <input>\n"
│       │   0x601225b671f3      4889c7         mov rdi, rax
│       │   0x601225b671f6      b800000000     mov eax, 0
│       │   0x601225b671fb      e8b0feffff     call sym.imp.printf     ; int printf(const char *format)
│       │   0x601225b67200      b800000000     mov eax, 0
│      ┌──< 0x601225b67205      e995010000     jmp 0x601225b6739f
│      │└─> 0x601225b6720a      488b45c0       mov rax, qword [var_40h]
│      │    0x601225b6720e      488b4008       mov rax, qword [rax + 8]
│      │    0x601225b67212      488945e0       mov qword [var_20h], rax
│      │    0x601225b67216      488b45e0       mov rax, qword [var_20h]
│      │    0x601225b6721a      4889c7         mov rdi, rax
│      │    0x601225b6721d      e87efeffff     call sym.imp.strlen     ; size_t strlen(const char *s)
│      │    0x601225b67222      4883c001       add rax, 1
│      │    0x601225b67226      488945e8       mov qword [var_18h], rax
│      │    0x601225b6722a      488b45e8       mov rax, qword [var_18h]
│      │    0x601225b6722e      4889c7         mov rdi, rax
│      │    0x601225b67231      e89afeffff     call sym.imp.malloc     ;  void *malloc(size_t size)
│      │    0x601225b67236      488945f0       mov qword [var_10h], rax
│      │    0x601225b6723a      488b45e8       mov rax, qword [var_18h]
│      │    0x601225b6723e      4889c7         mov rdi, rax
│      │    0x601225b67241      e88afeffff     call sym.imp.malloc     ;  void *malloc(size_t size)
│      │    0x601225b67246      488945f8       mov qword [var_8h], rax
│      │    0x601225b6724a      48c745d800..   mov qword [var_28h], 0
│      │┌─< 0x601225b67252      e9e1000000     jmp 0x601225b67338
│     ┌───> 0x601225b67257      488b55e0       mov rdx, qword [var_20h]
│     ╎││   0x601225b6725b      488b45d8       mov rax, qword [var_28h]
│     ╎││   0x601225b6725f      4801d0         add rax, rdx
│     ╎││   0x601225b67262      0fb600         movzx eax, byte [rax]
│     ╎││   0x601225b67265      0fbec0         movsx eax, al
│     ╎││   0x601225b67268      c1e003         shl eax, 3
│     ╎││   0x601225b6726b      89c6           mov esi, eax
│     ╎││   0x601225b6726d      488b55e0       mov rdx, qword [var_20h]
│     ╎││   0x601225b67271      488b45d8       mov rax, qword [var_28h]
│     ╎││   0x601225b67275      4801d0         add rax, rdx
│     ╎││   0x601225b67278      0fb600         movzx eax, byte [rax]
│     ╎││   0x601225b6727b      c0f805         sar al, 5
│     ╎││   0x601225b6727e      89c1           mov ecx, eax
│     ╎││   0x601225b67280      488b55e0       mov rdx, qword [var_20h]
│     ╎││   0x601225b67284      488b45d8       mov rax, qword [var_28h]
│     ╎││   0x601225b67288      4801d0         add rax, rdx
│     ╎││   0x601225b6728b      09ce           or esi, ecx
│     ╎││   0x601225b6728d      89f2           mov edx, esi
│     ╎││   0x601225b6728f      8810           mov byte [rax], dl
│     ╎││   0x601225b67291      488b55e0       mov rdx, qword [var_20h]
│     ╎││   0x601225b67295      488b45d8       mov rax, qword [var_28h]
│     ╎││   0x601225b67299      4801d0         add rax, rdx
│     ╎││   0x601225b6729c      0fb600         movzx eax, byte [rax]
│     ╎││   0x601225b6729f      0fbec0         movsx eax, al
│     ╎││   0x601225b672a2      83e001         and eax, 1
│     ╎││   0x601225b672a5      85c0           test eax, eax
│    ┌────< 0x601225b672a7      7446           je 0x601225b672ef
│    │╎││   0x601225b672a9      488b55e0       mov rdx, qword [var_20h]
│    │╎││   0x601225b672ad      488b45d8       mov rax, qword [var_28h]
│    │╎││   0x601225b672b1      4801d0         add rax, rdx
│    │╎││   0x601225b672b4      0fb610         movzx edx, byte [rax]
│    │╎││   0x601225b672b7      be33000000     mov esi, 0x33           ; '3' ; 51
│    │╎││   0x601225b672bc      488b4df0       mov rcx, qword [var_10h]
│    │╎││   0x601225b672c0      488b45d8       mov rax, qword [var_28h]
│    │╎││   0x601225b672c4      4801c8         add rax, rcx
│    │╎││   0x601225b672c7      21f2           and edx, esi
│    │╎││   0x601225b672c9      8810           mov byte [rax], dl
│    │╎││   0x601225b672cb      488b55e0       mov rdx, qword [var_20h]
│    │╎││   0x601225b672cf      488b45d8       mov rax, qword [var_28h]
│    │╎││   0x601225b672d3      4801d0         add rax, rdx
│    │╎││   0x601225b672d6      0fb610         movzx edx, byte [rax]
│    │╎││   0x601225b672d9      beccffffff     mov esi, 0xffffffcc     ; 4294967244
│    │╎││   0x601225b672de      488b4df8       mov rcx, qword [var_8h]
│    │╎││   0x601225b672e2      488b45d8       mov rax, qword [var_28h]
│    │╎││   0x601225b672e6      4801c8         add rax, rcx
│    │╎││   0x601225b672e9      21f2           and edx, esi
│    │╎││   0x601225b672eb      8810           mov byte [rax], dl
│   ┌─────< 0x601225b672ed      eb44           jmp 0x601225b67333
│   │└────> 0x601225b672ef      488b55e0       mov rdx, qword [var_20h]
│   │ ╎││   0x601225b672f3      488b45d8       mov rax, qword [var_28h]
│   │ ╎││   0x601225b672f7      4801d0         add rax, rdx
│   │ ╎││   0x601225b672fa      0fb610         movzx edx, byte [rax]
│   │ ╎││   0x601225b672fd      beccffffff     mov esi, 0xffffffcc     ; 4294967244
│   │ ╎││   0x601225b67302      488b4df0       mov rcx, qword [var_10h]
│   │ ╎││   0x601225b67306      488b45d8       mov rax, qword [var_28h]
│   │ ╎││   0x601225b6730a      4801c8         add rax, rcx
│   │ ╎││   0x601225b6730d      21f2           and edx, esi
│   │ ╎││   0x601225b6730f      8810           mov byte [rax], dl
│   │ ╎││   0x601225b67311      488b55e0       mov rdx, qword [var_20h]
│   │ ╎││   0x601225b67315      488b45d8       mov rax, qword [var_28h]
│   │ ╎││   0x601225b67319      4801d0         add rax, rdx
│   │ ╎││   0x601225b6731c      0fb610         movzx edx, byte [rax]
│   │ ╎││   0x601225b6731f      be33000000     mov esi, 0x33           ; '3' ; 51
│   │ ╎││   0x601225b67324      488b4df8       mov rcx, qword [var_8h]
│   │ ╎││   0x601225b67328      488b45d8       mov rax, qword [var_28h]
│   │ ╎││   0x601225b6732c      4801c8         add rax, rcx
│   │ ╎││   0x601225b6732f      21f2           and edx, esi
│   │ ╎││   0x601225b67331      8810           mov byte [rax], dl
│   │ ╎││   ; CODE XREF from main @ 0x601225b672ed(x)
│   └─────> 0x601225b67333      488345d801     add qword [var_28h], 1
│     ╎││   ; CODE XREF from main @ 0x601225b67252(x)
│     ╎│└─> 0x601225b67338      488b45d8       mov rax, qword [var_28h]
│     ╎│    0x601225b6733c      483b45e8       cmp rax, qword [var_18h]
│     └───< 0x601225b67340      0f8211ffffff   jb 0x601225b67257
│      │    0x601225b67346      488b45f0       mov rax, qword [var_10h]
│      │    0x601225b6734a      4889c6         mov rsi, rax
│      │    0x601225b6734d      488d05cc0c..   lea rax, obj.enc1       ; 0x601225b68020
│      │    0x601225b67354      4889c7         mov rdi, rax
│      │    0x601225b67357      e864fdffff     call sym.imp.strcmp     ; int strcmp(const char *s1, const char *s2)
│      │    0x601225b6735c      85c0           test eax, eax
│      │┌─< 0x601225b6735e      752b           jne 0x601225b6738b
│      ││   0x601225b67360      488b45f8       mov rax, qword [var_8h]
│      ││   0x601225b67364      4889c6         mov rsi, rax
│      ││   0x601225b67367      488d05d20c..   lea rax, obj.enc2       ; 0x601225b68040
│      ││   0x601225b6736e      4889c7         mov rdi, rax
│      ││   0x601225b67371      e84afdffff     call sym.imp.strcmp     ; int strcmp(const char *s1, const char *s2)
│      ││   0x601225b67376      85c0           test eax, eax
│     ┌───< 0x601225b67378      7511           jne 0x601225b6738b
│     │││   0x601225b6737a      488d05ef0c..   lea rax, str.congratz   ; 0x601225b68070 ; "congratz"
│     │││   0x601225b67381      4889c7         mov rdi, rax
│     │││   0x601225b67384      e807fdffff     call sym.imp.puts       ; int puts(const char *s)
│    ┌────< 0x601225b67389      eb0f           jmp 0x601225b6739a
│    │└─└─> 0x601225b6738b      488d05e70c..   lea rax, str.wrong      ; 0x601225b68079 ; "wrong"
│    │ │    0x601225b67392      4889c7         mov rdi, rax
│    │ │    0x601225b67395      e8f6fcffff     call sym.imp.puts       ; int puts(const char *s)
│    │ │    ; CODE XREF from main @ 0x601225b67389(x)
│    └────> 0x601225b6739a      b800000000     mov eax, 0
│      │    ; CODE XREF from main @ 0x601225b67205(x)
│      └──> 0x601225b6739f      c9             leave
└           0x601225b673a0      c3             ret
```

`obj.enc1`と`obj.enc2`で比較して違ったらそれぞれ`wrong`にジャンプしている。

obj.enc1とobj.enc2を確認する

```bash
[0x7b2a693ad440]> px 32 @ obj.enc1
- offset -      2021 2223 2425 2627 2829 2A2B 2C2D 2E2F  0123456789ABCDEF
0x56583d446020  0823 0303 1303 1303 0123 3113 11c8 03c8  .#.......#1.....
0x56583d446030  0313 01c8 1313 0313 1311 1323 0000 0000  ...........#....
[0x7b2a693ad440]> px 32 @ obj.enc2
- offset -      4041 4243 4445 4647 4849 4A4B 4C4D 4E4F  0123456789ABCDEF
0x56583d446040  0240 8008 0808 c8c8 8088 0880 8832 0832  .@...........2.2
0x56583d446050  8080 8032 0880 0808 4888 80c8 0075 7361  ...2....H....usa
[0x7b2a693ad440]>
```

判定ロジックを確認する
`0x601225b67257`付近で以下の計算が行われている

```c
// 入力文字を input[i] とする
val = (input[i] << 3) | (input[i] >> 5);
```

これは左に3ビット回転(ROL 3)させる操作。

回転後の`val`の値が偶数か奇数かによって処理が分岐する

- 偶数の場合 (`val & 1 == 0`)
  - `buffer1[i] = val & 0xCC (ビットマスク `11001100`)
  - `buffer2[i] = val & 0x33 (ビットマスク `00110011`)
- 奇数の場合 (`val & 1 != 0`)
  - `buffer1[i] = val & 0x33 (ビットマスク `00110011`)
  - `buffer2[i] = val & 0xcc (ビットマスク `11001100`)

つまり、入力文字のビットを分解して、`buffer1`と`buffer2`に振り分けている。

逆算する。

1. `obj.enc1`と`obj.enc2`を抽出
2. 各バイトについて、`onj.enc[1]`と`obj.enc2[i]を結合する
  - 偶数・奇数に関わらずただバイトを分けているだけなので、`enc1[i] | enc2[i]`で元に戻せる。
3. 結合した値を右に3ビット回転(ROR 3)させると元の入力文字になる。



ソルバーを書く
```py
# r2の出力から抽出したデータ
# enc1: 0x601225b68020 から 00 が出るまで
enc1 = [
    0x08, 0x23, 0x03, 0x03, 0x13, 0x03, 0x13, 0x03,
    0x01, 0x23, 0x31, 0x13, 0x11, 0xc8, 0x03, 0xc8,
    0x03, 0x13, 0x01, 0xc8, 0x13, 0x13, 0x03, 0x13,
    0x13, 0x11, 0x13, 0x23
]

# enc2: 0x601225b68040 から 00 が出るまで
enc2 = [
    0x02, 0x40, 0x80, 0x08, 0x08, 0x08, 0xc8, 0xc8,
    0x80, 0x88, 0x08, 0x80, 0x88, 0x32, 0x08, 0x32,
    0x80, 0x80, 0x80, 0x32, 0x08, 0x80, 0x08, 0x08,
    0x48, 0x88, 0x80, 0xc8
]

def ror(val, r_bits, max_bits=8):
    return ((val >> r_bits) | (val << (max_bits - r_bits))) & 0xFF

flag = ""

for b1, b2 in zip(enc1, enc2):
    # 1. ビット結合 (OR)
    combined = b1 | b2
    
    # 2. 右に3ビット回転 (ROR 3)
    decrypted_char = ror(combined, 3)
    
    flag += chr(decrypted_char)

print("Flag:", flag)
```

```bash
> python solve.py
Flag: Alpaca{y0u'r3_a_pr0_crack3r}
```

flag: `Alpaca{y0u'r3_a_pr0_crack3r}`
