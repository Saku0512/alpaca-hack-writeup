# RSA debug?

> すみません！RSA暗号を実装したんですけどバグが取れなくて…ちょっと見てもらえませんか？

[rsa-debug.tar.gz](https://alpacahack-prod.s3.ap-northeast-1.amazonaws.com/23c21e42-eb86-41c4-a9f2-91dd71430727/rsa-debug.tar.gz)が提供される

```
rsa-debug/
rsa-debug/problem.py
rsa-debug/output.txt
```

problem.py
```py
def my_pow(a, n, m):
    result = 1
    while n > 0:
        if n % 2 != 0:
            result = (result + a) % m # omg!
        a = (a + a) % m # oops!
        n = n // 2
    return result

from Crypto.Util.number import getPrime, bytes_to_long

p = getPrime(512)
q = getPrime(512)
N = p * q
phi = (p - 1) * (q - 1)
e = 0x101
d = my_pow(e, -1, phi)

with open('flag.txt', 'rb') as f:
    flag = bytes_to_long(f.read())


c = my_pow(flag, e, N)

print(f'N = {N}')
print(f'e = {e}')
print(f'c = {c}')

if my_pow(c, d, N) != flag:
    print('there is a bug i guess lol')
```

`my_pow`が脆弱

```py
def my_pow(a, n, m):
    result = 1
    while n > 0:
        if n % 2 != 0:
            result = (result + a) % m # ここが掛け算(+)になっている
        a = (a + a) % m               # ここも足し算(+)になっている
        n = n // 2
    return result
```

`my_pow(a,n,m) = (a * n) + 1 (mod m)`

初期値`result = 1`からスタートしているので`+1`がつく

$$
c = (flag * e) + 1 (mod N)
$$

flagについて解く

$$
flag = (c - 1) * e^{-1} (mod N)
$$

solve.py
```py
from Crypto.Util.number import long_to_bytes

# output.txt から値をコピー
N = 142152419619953056039030613006300356362328489550709389999387369748296278181079224756839343268342516013642694413617303501060708009652311527189475960379078488611592094334453807443536588862100338035073773947550237167141330041879985706663975671411606265404764401055491939553565105755943917581461262323010934286591
e = 257
c = 1032307400372525030420319173259503709384961767939821142794251896087430140750696054688678035256705431662987859651860033467060026426212901209540363

# 数式: c = (flag * e) + 1 mod N
# 変形: flag = (c - 1) * inverse(e, N) mod N

# 1. c から 1 を引く
c_minus_1 = c - 1

# 2. e の逆元を N を法として求める
# 通常RSAなら法はphiだが、今回は線形合同式なので法はN
e_inv = pow(e, -1, N)

# 3. 復号
m = (c_minus_1 * e_inv) % N

# 4. バイト列に変換して表示
flag = long_to_bytes(m)
print(flag)
```

flag: `TSGLIVE{g0od_Mult1plic4t10N-Algor1thm_6y_ru55iAn_Pea5ants}`
