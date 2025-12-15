# size limit

> 復号鍵も渡したんだし、これで誰でもメッセージを読めるよね！
>
> 注: この問題は TSG LIVE! CTF の過去問であり、フラグのフォーマットは TSGLIVE{...} です。フラグのフォーマットはフラグ提出フォームのプレースホルダーからも確認できます。

`output.txt`と`problem.py`が提供される

output.txt
```txt:output.txt
N = 65667982563395257456152578363358687414628050739860770903063206052667362178166666380390723634587933595241827767873104710537142458025201334420236653463444534018710274020834864080096247524541536313609304410859158429347482458882414275205742819080566766561312731091051276328620677195262137013588957713118640118673
e = 65537
c = 58443816925218320329602359198394095572237417576497896076618137604965419783093911328796166409276903249508047338019341719597113848471431947372873538253571717690982768328452282012361099369599755904288363602972252305949989677897650696581947849811037791349546750246816657184156675665729104603485387966759433211643
d = 14647215605104168233120807948419630020096019740227424951721591560155202409637919482865428659999792686501442518131270040719470657054982576354654918600616933355973824403026082055356501271036719280033851192012142309772828216012662939598631302504166489383155079998940570839539052860822636744356963005556392864865
```

problem.py
```py
#!/usr/bin/python3

from Crypto.Util.number import getPrime, bytes_to_long
import flag

assert(len(flag.flag) == 131)

p = getPrime(512)
q = getPrime(512)
N = p * q
phi = (p - 1) * (q - 1)
e = 0x10001
d = pow(e, -1, phi)

flag = bytes_to_long(flag.flag)


c = pow(flag, e, N)

print(f'N = {N}')
print(f'e = {e}')
print(f'c = {c}')
print(f'd = {d}')
```

秘密鍵`d`が与えられているため簡単に溶けそうだが、普通に復号すると
```txt
b'\x01\xf7\xb0\xaa\xec\x8f\xbe.\xf8\xa0\xb5\xfa`^\x8aA\xd7N\xa9\xf8qqyj\x90\xce\xd4_\xc58\x15\xb1\t\x1e)\xec\x98\xda\x9b8\xbd_\xc2\x01;\xa2\x91\xe7\xafI\xe9\xa4D\xf1\xfa\xf2\x82\xc5c"\x0c~C:\x90\x99\xeb\xab\xd4\x86\xe7\x01\x1e\xa3\xb4\x9a>\xfeY\x94\xc7EP\x08A\xd1\x12\xa7\xfb\xbf\xe0\x10*\xc5\xcfN0Al\x92!~]^\xed\x02\x1c\xb3\x8ab\xbb\x9d\xcf\xae\xc9\xbe\x8e\x8a\x98:-\xe1N3\xe6\xa8\xef\x12'
```
となりflagではないものがある。

`problem.py`を見ると、以下のassert 文がある。
```py
assert(len(flag.flag) == 131)
```
また、鍵生成のパラメータは以下の通り。

- $p,q$: 512 bit primes
- $N: 512 \times 512  = 1024 bit$

ここで、フラグ（平文 $m$）のサイズと $N$ のサイズを比較する。
- 平文 $m$: $131 \text{ bytes} \times 8 = 1048 \text{ bits}$
- $N$: $1024 \text{ bits}$

RSA暗号の復号 $m \equiv c^d \pmod N$ が正しく元の平文に戻る条件は $m < N$ である。
しかし、今回は $m > N$ となっているため、通常の復号を行うと $N$ で割った余り（ $m'$ とする）しか得られない。

真の平文 $m$ と、復号結果 $m'$ の間には以下の関係がある。

$$
m = m' + k \times N \quad (k \ge 0)
$$

ここで、$m$ と $N$ のビット差に注目する。$1048 - 1024 = 24 \text{ bits}$したがって、係数 $k$ はおよそ $2^{24}$ (約1677万) 程度の範囲に収まる。

これは十分に小さい値なので、全探索する。

```py
from Crypto.Util.number import long_to_bytes

# output.txt から値をコピー
N = 65667982563395257456152578363358687414628050739860770903063206052667362178166666380390723634587933595241827767873104710537142458025201334420236653463444534018710274020834864080096247524541536313609304410859158429347482458882414275205742819080566766561312731091051276328620677195262137013588957713118640118673
e = 65537
c = 58443816925218320329602359198394095572237417576497896076618137604965419783093911328796166409276903249508047338019341719597113848471431947372873538253571717690982768328452282012361099369599755904288363602972252305949989677897650696581947849811037791349546750246816657184156675665729104603485387966759433211643
d = 14647215605104168233120807948419630020096019740227424951721591560155202409637919482865428659999792686501442518131270040719470657054982576354654918600616933355973824403026082055356501271036719280033851192012142309772828216012662939598631302504166489383155079998940570839539052860822636744356963005556392864865

# 1. まず普通に復号する (m mod N が得られる)
m_prime = pow(c, d, N)

# 2. フラグは m = m_prime + k * N の形をしているはず
# k を小さい値から探索する
print("Searching for k...")

# kの範囲は適当に大きく取っている
for k in range(2**26):
    candidate = m_prime + k * N

    # バイト列に変換してみる
    try:
        flag_bytes = long_to_bytes(candidate)

        # TSGLIVEのフラグフォーマットを含んでいるかチェック
        # (TSGLIVE{...} もしくは単に読み取れる文字列か)
        if b'TSGLIVE{' in flag_bytes:
            print(f"\nFound flag with k={k}!")
            print(flag_bytes.decode())
            break

    except Exception:
        # 変換できない場合などは無視
        pass

    if k % 1000000 == 0:
        print(f"Checked up to k={k}...")

```

```bash
> python solve.py
Searching for k...
Checked up to k=0...
Checked up to k=1000000...
Checked up to k=2000000...
Checked up to k=3000000...
Checked up to k=4000000...
Checked up to k=5000000...
Checked up to k=6000000...
Checked up to k=7000000...
Checked up to k=8000000...
Checked up to k=9000000...
Checked up to k=10000000...
Checked up to k=11000000...
Checked up to k=12000000...
Checked up to k=13000000...
Checked up to k=14000000...
Checked up to k=15000000...

Found flag with k=15128635!
TSGLIVE{Tttthhhhhiiiiiiisssss iiiiiiiiiisssss aaaaaaaaaaaaaa tooooooooooooooooooooo looooooooooooooooong fllllaaaaaaaaaaaaaaaaaag!}
```

flag: `TSGLIVE{Tttthhhhhiiiiiiisssss iiiiiiiiiisssss aaaaaaaaaaaaaa tooooooooooooooooooooo looooooooooooooooong fllllaaaaaaaaaaaaaaaaaag!}`
