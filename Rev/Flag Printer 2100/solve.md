# Flag Printer 2100

> 75 年後にフラグを出力するプログラムを作ったので、2100 年まで待ってね!

print_flagというファイルが提供される

```bash
> file print_flag
print_flag: ELF 64-bit LSB pie executable, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, BuildID[sha1]=4779e9456734b25b443894084d1ef12e88a94719, for GNU/Linux 3.2.0, not stripped
```
実行可能ファイルと分かる。

実行する
```bash
> ./print_flag
I'll sleep for 75 years...

```

sleepしてるっぽい。

radare2を使って解析する。

```bash
> r2 -d ./print_flag
[0x70d2243ae440]> aaa
INFO: Analyze all flags starting with sym. and entry0 (aa)
INFO: Analyze imports (af@@@i)
INFO: Analyze entrypoint (af@ entry0)
INFO: Analyze symbols (af@@@s)
INFO: Analyze all functions arguments/locals (afva@@@F)
INFO: Analyze function calls (aac)
INFO: Analyze len bytes of instructions for references (aar)
INFO: Finding and parsing C++ vtables (avrr)
INFO: Analyzing methods (af @@ method.*)
INFO: Recovering local variables (afva@@@F)
INFO: Skipping type matching analysis in debugger mode (aaft)
INFO: Propagate noreturn information (aanr)
INFO: Use -AA or aaaa to perform additional experimental analysis
[0x70d2243ae440]> afl
0x55da8b348080    1     10 sym.imp.putchar
0x55da8b348090    1     10 sym.imp.puts
0x55da8b3480a0    1     10 sym.imp.__stack_chk_fail
0x55da8b3480b0    1     10 sym.imp.sleep
0x55da8b3480c0    1     38 entry0
0x55da8b34afd8    1   4137 fcn.55da8b34afd8
0x55da8b3480f0    4     34 sym.deregister_tm_clones
0x55da8b348120    4     51 sym.register_tm_clones
0x55da8b348160    5     54 entry.fini0
0x55da8b348070    1     10 fcn.55da8b348070
0x55da8b3481a0    1      9 entry.init0
0x55da8b3481b0   10   2074 sym.show_flag
0x55da8b348a04    1     13 sym._fini
0x55da8b3489d0    1     50 main
0x55da8b348000    3     27 sym._init
0x70d22439387b    3     40 fcn.70d22439387b
0x70d22439f6a0    3    160 fcn.70d22439f6a0
0x70d2243ac150    5    174 fcn.70d2243ac150
0x70d22439cc70   23    415 fcn.70d22439cc70
0x70d22439f360    3    162 fcn.70d22439f360
0x70d22439ed40  107   1503 fcn.70d22439ed40
0x70d2243b5200    1     74 fcn.70d2243b5200
0x70d224394020    3    148 fcn.70d224394020
0x70d224396660    8    194 fcn.70d224396660
0x70d2243940c0    5     96 fcn.70d2243940c0
0x70d224394120    7    104 fcn.70d224394120
0x70d2243b51b0    1     79 fcn.70d2243b51b0
0x70d224394320   21    340 fcn.70d224394320
0x70d224394500   17    225 fcn.70d224394500
0x70d2243a2ac0   57   1065 fcn.70d2243a2ac0
0x70d2243a98b0    8    152 fcn.70d2243a98b0
0x70d2243a96f0    5     49 fcn.70d2243a96f0
0x70d2243a2820   14    187 fcn.70d2243a2820
0x70d2243c79f8    1      8 fcn.70d2243c79f8
0x70d2243c79f0    1      8 fcn.70d2243c79f0
0x70d2243a2f20   30    407 fcn.70d2243a2f20
0x70d224395450    5     64 fcn.70d224395450
0x70d224395440    1      1 fcn.70d224395440
0x70d2243a66e0    6    117 fcn.70d2243a66e0
0x70d2243b48e0   18    187 fcn.70d2243b48e0
0x70d224394610  199   3393 fcn.70d224394610
0x70d22439a560  147   3213 fcn.70d22439a560
0x70d2243b5ac0   27    459 fcn.70d2243b5ac0
0x70d2243b7c60   13    380 fcn.70d2243b7c60
0x70d2243980c0   24    359 fcn.70d2243980c0
0x70d224398010   10    162 fcn.70d224398010
0x70d2243b6420   10    526 fcn.70d2243b6420
0x70d224397140   11    200 fcn.70d224397140
0x70d2243b7e00   27    438 fcn.70d2243b7e00
0x70d22439eba0   18    318 fcn.70d22439eba0
0x70d2243b5ab0    1      8 fcn.70d2243b5ab0
0x70d2243b4e50   14    160 fcn.70d2243b4e50
0x70d224396bb0    1     31 fcn.70d224396bb0
0x70d2243b5ca0   18    187 fcn.70d2243b5ca0
0x70d224396620    3     51 fcn.70d224396620
0x70d2243b4fa0    3     32 fcn.70d2243b4fa0
0x70d2243a4f00    9     98 fcn.70d2243a4f00
0x70d224396bd0    3     57 fcn.70d224396bd0
0x70d224393f70    9    152 fcn.70d224393f70
0x70d224396ee0   19    266 fcn.70d224396ee0
0x70d2243b57e0   40    681 fcn.70d2243b57e0
0x70d2243b4d70    6    100 fcn.70d2243b4d70
0x70d2243b4e10    3     29 fcn.70d2243b4e10
0x70d2243b4c80    3     30 fcn.70d2243b4c80
0x70d2243b4de0    3     35 fcn.70d2243b4de0
0x70d2243a9730   10    168 fcn.70d2243a9730
0x70d2243974b0   51    823 fcn.70d2243974b0
0x70d2243b4b60    3     30 fcn.70d2243b4b60
0x70d2243b4c60    1     18 fcn.70d2243b4c60
0x70d22439f410    3    162 fcn.70d22439f410
0x70d2243b6640   17    337 fcn.70d2243b6640
0x70d2243b5d70   27    790 fcn.70d2243b5d70
0x70d224397220   40    410 fcn.70d224397220
0x70d2243ac510   12     80 fcn.70d2243ac510
0x70d224398250   12    239 fcn.70d224398250
0x70d2243b7c00    3     61 fcn.70d2243b7c00
0x70d224398350   40    697 fcn.70d224398350
0x70d2243abb20   27    872 fcn.70d2243abb20
0x70d224398640   32    451 fcn.70d224398640
0x70d22439d420   50    964 fcn.70d22439d420
0x70d2243b4f00   10    141 fcn.70d2243b4f00
0x70d224397f30   11    196 fcn.70d224397f30
0x70d22439d0d0    8     88 fcn.70d22439d0d0
0x70d22439d380    6    140 fcn.70d22439d380
0x70d2243c7a10    7   1618 fcn.70d2243c7a10
0x70d224397400    9    148 fcn.70d224397400
0x70d2243b4fc0    3     32 fcn.70d2243b4fc0
0x70d224398c70   34    462 fcn.70d224398c70
0x70d2243a28f0   10    174 fcn.70d2243a28f0
0x70d2243a3480   16    219 fcn.70d2243a3480
0x70d224398e70  276   5721 fcn.70d224398e70
0x70d224397820   69   1515 fcn.70d224397820
0x70d224398830    9     95 fcn.70d224398830
0x70d2243a60c0   39    759 fcn.70d2243a60c0
0x70d224397e40   10    170 fcn.70d224397e40
0x70d22439b260   21    315 fcn.70d22439b260
0x70d22439f600    3    158 fcn.70d22439f600
0x70d22439d140    6     64 fcn.70d22439d140
0x70d22439b3b0  119   3082 fcn.70d22439b3b0
0x70d224396730   69   1060 fcn.70d224396730
0x70d224394190    9    247 fcn.70d224394190
0x70d22439cba0   10    200 fcn.70d22439cba0
0x70d2243b4fe0    6    109 fcn.70d2243b4fe0
0x70d2243b3080   88   1493 fcn.70d2243b3080
0x70d22439d840   11    278 fcn.70d22439d840
0x70d22439dad0    1     32 fcn.70d22439dad0
0x70d2243b3bc0    7    109 fcn.70d2243b3bc0
0x70d2243a63e0    3     70 fcn.70d2243a63e0
0x70d22439dc50   14    160 fcn.70d22439dc50
0x70d2243954a0   21    332 fcn.70d2243954a0
0x70d224395680  163   3814 fcn.70d224395680
0x70d2243a4f80  122   2233 fcn.70d2243a4f80
0x70d2243a2220    3     28 fcn.70d2243a2220
0x70d22439f9f0  453   8405 fcn.70d22439f9f0
0x70d2243b4400   81   1194 fcn.70d2243b4400
0x70d2243a3ed0   16    291 fcn.70d2243a3ed0
0x70d22439dfe0   13    348 fcn.70d22439dfe0
0x70d2243ac640   59   1831 fcn.70d2243ac640
0x70d22439daf0   16    342 fcn.70d22439daf0
0x70d2243a3bb0   23    417 fcn.70d2243a3bb0
0x70d2243a4010   11    283 fcn.70d2243a4010
0x70d224393f00   38   1047 fcn.70d224393f00
0x70d2243b5180    1      8 fcn.70d2243b5180
0x70d2243b5150    4     30 fcn.70d2243b5150
0x70d22439f740   15    264 fcn.70d22439f740
0x70d22439c030  171   3445 fcn.70d22439c030
0x70d2243a9b80   25    623 fcn.70d2243a9b80
0x70d22439f910    3    212 fcn.70d22439f910
0x70d2243a6440   37    636 fcn.70d2243a6440
0x70d22439f860    4     48 fcn.70d22439f860
0x70d2243a9e10   21    502 fcn.70d2243a9e10
0x70d2243acd90   23    501 fcn.70d2243acd90
0x70d2243a29c0   14    193 fcn.70d2243a29c0
0x70d2243ac270    1     48 fcn.70d2243ac270
0x70d2243b5060    7     73 fcn.70d2243b5060
0x70d2243ac2a0   24   1659 fcn.70d2243ac2a0
0x70d2243a30c0    3     61 fcn.70d2243a30c0
0x70d2243c7a00    1     16 fcn.70d2243c7a00
0x70d2243a3320    8    202 fcn.70d2243a3320
0x70d2243c79e8    1      8 fcn.70d2243c79e8
0x70d2243a37f0    5    203 fcn.70d2243a37f0
0x70d2243a24f0   35    778 fcn.70d2243a24f0
0x70d2243aa010    9    320 fcn.70d2243aa010
0x70d2243a2250   54   1261 fcn.70d2243a2250
0x70d2243a46e0   30    287 fcn.70d2243a46e0
0x70d22439d1c0   36    397 fcn.70d22439d1c0
0x70d2243a4830    6    123 fcn.70d2243a4830
0x70d22439f560    3    158 fcn.70d22439f560
0x70d2243a5940   17    237 fcn.70d2243a5940
0x70d2243a5a30   75   1618 fcn.70d2243a5a30
0x70d22439d040    7    131 fcn.70d22439d040
0x70d2243a3da0   33    676 fcn.70d2243a3da0
0x70d2243a4140    5     58 fcn.70d2243a4140
0x70d2243a76d0   51    599 fcn.70d2243a76d0
0x70d2243a7060   14    394 fcn.70d2243a7060
0x70d2243a7a30   53   1459 fcn.70d2243a7a30
0x70d2243a48e0    1     28 fcn.70d2243a48e0
0x70d2243a48c0    1     25 fcn.70d2243a48c0
0x70d2243a7940   11    225 fcn.70d2243a7940
0x70d2243a7200   89   1150 fcn.70d2243a7200
0x70d2243a6a80   19    294 fcn.70d2243a6a80
0x70d2243a6bc0   24    311 fcn.70d2243a6bc0
0x70d2243a6f10    8    128 fcn.70d2243a6f10
0x70d2243a8010  154   5406 fcn.70d2243a8010
0x70d2243a5930    1      8 fcn.70d2243a5930
0x70d2243aa150    6    152 fcn.70d2243aa150
0x70d2243aa1f0    6    126 fcn.70d2243aa1f0
0x70d2243aa270    1     64 fcn.70d2243aa270
0x70d2243abea0   15    175 fcn.70d2243abea0
0x70d2243ab8b0   11    394 fcn.70d2243ab8b0
0x70d2243ab0f0   52   2937 fcn.70d2243ab0f0
0x70d2243aa700   26    396 fcn.70d2243aa700
0x70d2243aa8b0   26    353 fcn.70d2243aa8b0
0x70d2243aaa20   13    486 fcn.70d2243aaa20
0x70d2243aa2b0    1     20 fcn.70d2243aa2b0
0x70d2243b5190    3     32 fcn.70d2243b5190
0x70d2243ac0b0    5     64 fcn.70d2243ac0b0
0x70d2243abf60    9    149 fcn.70d2243abf60
0x70d2243aba80    4    156 fcn.70d2243aba80
0x70d2243ac000   13    159 fcn.70d2243ac000
0x70d2243a99d0   15    392 fcn.70d2243a99d0
0x70d2243ac2c0    9    201 fcn.70d2243ac2c0
0x70d2243b4a30    1      7 fcn.70d2243b4a30
0x70d2243b4a90    4    162 fcn.70d2243b4a90
0x70d2243b5270   10    387 fcn.70d2243b5270
0x70d2243b9a80    3     30 fcn.70d2243b9a80
0x70d2243b4bb0    3     32 fcn.70d2243b4bb0
0x70d2243b4e30    3     32 fcn.70d2243b4e30
0x70d2243acfc0   11    496 fcn.70d2243acfc0
0x70d2243a4900   84   1108 fcn.70d2243a4900
0x70d2243a2a90    1     42 fcn.70d2243a2a90
0x70d2243b3050    3     38 fcn.70d2243b3050
0x70d2243a9630    3     16 fcn.70d2243a9630
0x70d2243b3830   12    120 fcn.70d2243b3830
0x70d2243b3960   13    200 fcn.70d2243b3960
0x70d2243ae030    8    152 fcn.70d2243ae030
0x70d2243988a0   36    966 fcn.70d2243988a0
0x70d2243aefb0  105   1659 fcn.70d2243aefb0
0x70d224397000   17    285 fcn.70d224397000
0x70d22439dd00   32    709 fcn.70d22439dd00
0x70d2243ae570   25    537 fcn.70d2243ae570
0x70d2243944a0    1     65 fcn.70d2243944a0
0x70d2243b7a80   15    371 fcn.70d2243b7a80
0x70d2243b9870   10    510 fcn.70d2243b9870
0x70d2243b54b0   53    743 fcn.70d2243b54b0
0x70d2243953d0    5    109 fcn.70d2243953d0
0x70d2243a58b0    7     99 fcn.70d2243a58b0
0x70d2243b9650   19    494 fcn.70d2243b9650
0x70d2243a33f0    3    144 fcn.70d2243a33f0
0x70d2243a35f0   20    462 fcn.70d2243a35f0
0x70d2243a41f0    5    314 fcn.70d2243a41f0
0x70d2243ac100    1     57 fcn.70d2243ac100
0x70d2243add90    7    177 fcn.70d2243add90
0x70d2243ae7b0   27    519 fcn.70d2243ae7b0
0x70d2243aea50    3    142 fcn.70d2243aea50
0x70d2243a4190    1     85 fcn.70d2243a4190
0x70d2243ab980    8     55 fcn.70d2243ab980
0x70d2243ade60   22    456 fcn.70d2243ade60
0x70d224396c10   37    671 fcn.70d224396c10
0x70d2243a3570   10    113 fcn.70d2243a3570
0x70d2243a9660    6    130 fcn.70d2243a9660
0x70d2243b4b40    3     30 fcn.70d2243b4b40
0x70d2243a97e0    7    189 fcn.70d2243a97e0
0x70d224394290    6    132 fcn.70d224394290
0x70d2243a3e80    6     66 fcn.70d2243a3e80
0x70d2243a38d0   26    516 fcn.70d2243a38d0
0x70d2243b4250   26    587 fcn.70d2243b4250
0x70d2243ac570   10    180 fcn.70d2243ac570
0x70d2243ac3a0    3    270 fcn.70d2243ac3a0
0x70d22439f890    6    104 fcn.70d22439f890
0x70d2243ade50    1      1 fcn.70d2243ade50
0x70d2243aeae0   19    337 fcn.70d2243aeae0
0x70d2243aeee0    7    203 fcn.70d2243aeee0
0x70d2243aec50    3     72 fcn.70d2243aec50
0x70d2243ae0d0    4     75 fcn.70d2243ae0d0
0x70d2243ae140   31    707 fcn.70d2243ae140
0x70d2243aece0   26    492 fcn.70d2243aece0
0x70d2243b7c50    6     50 fcn.70d2243b7c50
0x70d2243b5400   12    155 fcn.70d2243b5400
0x70d2243ab9e0   16    141 fcn.70d2243ab9e0
0x70d2243af680   14    235 fcn.70d2243af680
0x70d2243a4d90   13    253 fcn.70d2243a4d90
0x70d2243aa2d0   27   1051 fcn.70d2243aa2d0
0x70d2243ae120    1     32 fcn.70d2243ae120
0x70d2243b4bd0    1     21 fcn.70d2243b4bd0
0x70d2243b4bf0    6     89 fcn.70d2243b4bf0
0x70d2243b39a0    3     38 fcn.70d2243b39a0
0x70d2243b39d0   11    147 fcn.70d2243b39d0
0x70d2243b4ca0    7    106 fcn.70d2243b4ca0
0x70d2243b36d0    8    182 fcn.70d2243b36d0
0x70d2243b3a70   13    193 fcn.70d2243b3a70
0x70d2243b4b80    3     34 fcn.70d2243b4b80
0x70d2243b3b60    1     40 fcn.70d2243b3b60
0x70d2243b3b90    3     44 fcn.70d2243b3b90
0x70d2243b5110    1     21 fcn.70d2243b5110
0x70d2243b3c40    6     99 fcn.70d2243b3c40
0x70d2243b3cb0   29    407 fcn.70d2243b3cb0
[0x55da8b3489d0]>
```

`sym.show_flag`をみてみる

```bash
[0x55da8b3481b0]> pdf
Do you want to print 459 lines? (y/N) y
        ╎   ; CALL XREF from main @ 0x55da8b3489f6(x)
┌ 2074: sym.show_flag ();
│ afv: vars(52:sp[0x40..0x330])
│       ╎   0x55da8b3481b0      f30f1efa       endbr64
│       ╎   0x55da8b3481b4      4157           push r15
│       ╎   0x55da8b3481b6      660fefc9       pxor xmm1, xmm1
│       ╎   0x55da8b3481ba      66450fefd2     pxor xmm10, xmm10
│       ╎   0x55da8b3481bf      4156           push r14
│       ╎   0x55da8b3481c1      4155           push r13
│       ╎   0x55da8b3481c3      4154           push r12
│       ╎   0x55da8b3481c5      55             push rbp
│       ╎   0x55da8b3481c6      53             push rbx
│       ╎   0x55da8b3481c7      4881ec0803..   sub rsp, 0x308
│       ╎   0x55da8b3481ce      660f6f055a..   movdqa xmm0, xmmword [0x55da8b349030] ; [0x55da8b349030:16]=-1
│       ╎   0x55da8b3481d6      64488b0425..   mov rax, qword fs:[0x28]
│       ╎   0x55da8b3481df      48898424f8..   mov qword [var_2f8h], rax
│       ╎   0x55da8b3481e7      31c0           xor eax, eax
│       ╎   0x55da8b3481e9      488d9c2470..   lea rbx, [var_270h]
│       ╎   0x55da8b3481f1      48b8416c70..   movabs rax, 0x6148616361706c41 ; 'AlpacaHa'
│       ╎   0x55da8b3481fb      0f29442470     movaps xmmword [var_70h], xmm0
│       ╎   0x55da8b348200      660f6f0538..   movdqa xmm0, xmmword [0x55da8b349040] ; [0x55da8b349040:16]=-1
│       ╎   0x55da8b348208      0f29842480..   movaps xmmword [var_80h], xmm0
│       ╎   0x55da8b348210      660f6f0538..   movdqa xmm0, xmmword [0x55da8b349050] ; [0x55da8b349050:16]=-1
│       ╎   0x55da8b348218      0f29842490..   movaps xmmword [var_90h], xmm0
│       ╎   0x55da8b348220      660f6f0538..   movdqa xmm0, xmmword [0x55da8b349060] ; [0x55da8b349060:16]=-1
│       ╎   0x55da8b348228      0f298424a0..   movaps xmmword [var_a0h], xmm0
│       ╎   0x55da8b348230      660f6f0538..   movdqa xmm0, xmmword [0x55da8b349070] ; [0x55da8b349070:16]=-1
│       ╎   0x55da8b348238      0f298424b0..   movaps xmmword [var_b0h], xmm0
│       ╎   0x55da8b348240      660f6f0538..   movdqa xmm0, xmmword [0x55da8b349080] ; [0x55da8b349080:16]=-1
│       ╎   0x55da8b348248      0f298424c0..   movaps xmmword [var_c0h], xmm0
│       ╎   0x55da8b348250      660f6f0538..   movdqa xmm0, xmmword [0x55da8b349090] ; [0x55da8b349090:16]=-1
│       ╎   0x55da8b348258      0f298424d0..   movaps xmmword [var_d0h], xmm0
│       ╎   0x55da8b348260      660f6f0538..   movdqa xmm0, xmmword [0x55da8b3490a0] ; [0x55da8b3490a0:16]=-1
│       ╎   0x55da8b348268      0f298424e0..   movaps xmmword [var_e0h], xmm0
│       ╎   0x55da8b348270      660f6f0538..   movdqa xmm0, xmmword [str.R_vjt] ; [0x55da8b3490b0:16]=-1 ; "R\vjt\xf3\x82P{L~\x98\x8b\x9d\x96\xb7h\x99\xc1Y@\xd9E\x1c\xc3F\xe6\x14\xb7;7\xa22\x8a\x91\ubcba\xe1\xd2G\xbe\xceeG\xda\xdeO;)~\xdd\x17"
│       ╎   0x55da8b348278      0f298424f0..   movaps xmmword [var_f0h], xmm0
│       ╎   0x55da8b348280      660f6f0538..   movdqa xmm0, xmmword [0x55da8b3490c0] ; [0x55da8b3490c0:16]=-1
│       ╎   0x55da8b348288      0f29842400..   movaps xmmword [var_100h], xmm0
│       ╎   0x55da8b348290      660f6f0538..   movdqa xmm0, xmmword [0x55da8b3490d0] ; [0x55da8b3490d0:16]=-1
│       ╎   0x55da8b348298      0f29842410..   movaps xmmword [var_110h], xmm0
│       ╎   0x55da8b3482a0      660f6f0538..   movdqa xmm0, xmmword [0x55da8b3490e0] ; [0x55da8b3490e0:16]=-1
│       ╎   0x55da8b3482a8      0f29842420..   movaps xmmword [var_120h], xmm0
│       ╎   0x55da8b3482b0      660f6f0538..   movdqa xmm0, xmmword [0x55da8b3490f0] ; [0x55da8b3490f0:16]=-1
│       ╎   0x55da8b3482b8      660f6f25a0..   movdqa xmm4, xmmword [0x55da8b349160] ; [0x55da8b349160:16]=-1
│       ╎   0x55da8b3482c0      48898424b0..   mov qword [var_2b0h], rax
│       ╎   0x55da8b3482c8      b8636b0000     mov eax, 0x6b63         ; 'ck'
│       ╎   0x55da8b3482cd      660f6f359b..   movdqa xmm6, xmmword [0x55da8b349170] ; [0x55da8b349170:16]=-1
│       ╎   0x55da8b3482d5      0f29842430..   movaps xmmword [var_130h], xmm0
│       ╎   0x55da8b3482dd      660f6f051b..   movdqa xmm0, xmmword [0x55da8b349100] ; [0x55da8b349100:16]=-1
│       ╎   0x55da8b3482e5      66440f6fc4     movdqa xmm8, xmm4
│       ╎   0x55da8b3482ea      660f6f1d3e..   movdqa xmm3, xmmword [0x55da8b349130] ; [0x55da8b349130:16]=-1
│       ╎   0x55da8b3482f2      66898424b8..   mov word [var_2b8h], ax
│       ╎   0x55da8b3482fa      660f71d608     psrlw xmm6, 8
│       ╎   0x55da8b3482ff      660f6f1539..   movdqa xmm2, xmmword [0x55da8b349140] ; [0x55da8b349140:16]=-1
│       ╎   0x55da8b348307      0f29842440..   movaps xmmword [var_140h], xmm0
│       ╎   0x55da8b34830f      660f6f05f9..   movdqa xmm0, xmmword [0x55da8b349110] ; [0x55da8b349110:16]=-1
│       ╎   0x55da8b348317      488b05320e..   mov rax, qword [0x55da8b349150] ; [0x55da8b349150:8]=0x5000000000000000
│       ╎   0x55da8b34831e      c68424ba02..   mov byte [var_2bah], 0x80 ; [0x80:1]=255 ; 128
│       ╎   0x55da8b348326      0f29842450..   movaps xmmword [var_150h], xmm0
│       ╎   0x55da8b34832e      660f6f05ea..   movdqa xmm0, xmmword [0x55da8b349120] ; [0x55da8b349120:16]=-1
│       ╎   0x55da8b348336      48c78424db..   mov qword [var_2dbh], 0
│       ╎   0x55da8b348342      0f29842460..   movaps xmmword [var_160h], xmm0
│       ╎   0x55da8b34834a      660fefc0       pxor xmm0, xmm0
│       ╎   0x55da8b34834e      0f118424bb..   movups xmmword [var_2bbh], xmm0
│       ╎   0x55da8b348356      660f6fac24..   movdqa xmm5, xmmword [var_2b0h]
│       ╎   0x55da8b34835f      0f118424cb..   movups xmmword [var_2cbh], xmm0
│       ╎   0x55da8b348367      660f6f8424..   movdqa xmm0, xmmword [var_2b0h]
│       ╎   0x55da8b348370      660fdbec       pand xmm5, xmm4
│       ╎   0x55da8b348374      c68424e702..   mov byte [var_2e7h], 0
│       ╎   0x55da8b34837c      660f71d008     psrlw xmm0, 8
│       ╎   0x55da8b348381      660f67e9       packuswb xmm5, xmm1
│       ╎   0x55da8b348385      c78424e302..   mov dword [var_2e3h], 0
│       ╎   0x55da8b348390      660f67c1       packuswb xmm0, xmm1
│       ╎   0x55da8b348394      66440fdbc5     pand xmm8, xmm5
│       ╎   0x55da8b348399      0f295c2430     movaps xmmword [var_30h], xmm3
│       ╎   0x55da8b34839e      660fdbe0       pand xmm4, xmm0
│       ╎   0x55da8b3483a2      660f71d508     psrlw xmm5, 8
│       ╎   0x55da8b3483a7      66440f67c1     packuswb xmm8, xmm1
│       ╎   0x55da8b3483ac      48898424e8..   mov qword [var_2e8h], rax
│       ╎   0x55da8b3483b4      660f71d008     psrlw xmm0, 8
│       ╎   0x55da8b3483b9      660f67e9       packuswb xmm5, xmm1
│       ╎   0x55da8b3483bd      660f67e1       packuswb xmm4, xmm1
│       ╎   0x55da8b3483c1      0f29542440     movaps xmmword [var_40h], xmm2
│       ╎   0x55da8b3483c6      660f67c6       packuswb xmm0, xmm6
│       ╎   0x55da8b3483ca      66450f6fd8     movdqa xmm11, xmm8
│       ╎   0x55da8b3483cf      660f6ffc       movdqa xmm7, xmm4
│       ╎   0x55da8b3483d3      0f295c2450     movaps xmmword [var_50h], xmm3
│       ╎   0x55da8b3483d8      66410f68e2     punpckhbw xmm4, xmm10
│       ╎   0x55da8b3483dd      66450f68c2     punpckhbw xmm8, xmm10
│       ╎   0x55da8b3483e2      66440f6fcd     movdqa xmm9, xmm5
│       ╎   0x55da8b3483e7      0f29542460     movaps xmmword [var_60h], xmm2
│       ╎   0x55da8b3483ec      66440f6fe0     movdqa xmm12, xmm0
│       ╎   0x55da8b3483f1      660f6ff0       movdqa xmm6, xmm0
│       ╎   0x55da8b3483f5      66450f60da     punpcklbw xmm11, xmm10
│       ╎   0x55da8b3483fa      66410f60fa     punpcklbw xmm7, xmm10
│       ╎   0x55da8b3483ff      66450f60e2     punpcklbw xmm12, xmm10
│       ╎   0x55da8b348404      66410f68f2     punpckhbw xmm6, xmm10
│       ╎   0x55da8b348409      66450f60ca     punpcklbw xmm9, xmm10
│       ╎   0x55da8b34840e      66410f68ea     punpckhbw xmm5, xmm10
│       ╎   0x55da8b348413      660f6fc4       movdqa xmm0, xmm4
│       ╎   0x55da8b348417      66450f6fd0     movdqa xmm10, xmm8
│       ╎   0x55da8b34841c      660f69c1       punpckhwd xmm0, xmm1
│       ╎   0x55da8b348420      66440f6fee     movdqa xmm13, xmm6
│       ╎   0x55da8b348425      66440f69d1     punpckhwd xmm10, xmm1
│       ╎   0x55da8b34842a      660f71f508     psllw xmm5, 8
│       ╎   0x55da8b34842f      66440f69e9     punpckhwd xmm13, xmm1
│       ╎   0x55da8b348434      66410f72f218   pslld xmm10, 0x18
```

```bash
[0x73ed33a3c440]> axt sym.show_flag
main 0x5a85693789f6 [CALL:--x] call sym.show_flag
[0x73ed33a3c440]> axt sym.imp.sleep
main 0x5a85693789ec [CALL:--x] call sym.imp.sleep
[0x73ed33a3c440]>
```

show_flagの後にsleepが呼ばれている。

sleepの後におそらくflagがあると想像できる。

```bash
[0x5a85693780b0]> db sym.imp.sleep
[0x5a85693780b0]> dc
I'll sleep for 75 years...
INFO: hit breakpoint at: 0x5a85693780b0
[0x5a85693780b0]> wa ret
INFO: Written 1 byte(s) (ret) = wx c3 @ 0x5a85693780b0
[0x5a85693780b0]> dc
Alpaca{G00d_Morning_AlpacaH4ck!}
(4063) Process exited with status=0x0
[0x73ed338e8295]>
```

sleep呼び出しをretにして回避した。

flag: `Alpaca{G00d_Morning_AlpacaH4ck!}`
