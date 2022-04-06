---
title: "【演算法筆記】Pollard's p - 1 Algorithm"
date: 2018-02-22
categories:
- 演算法
tags:
- algorithm
---

有一個合數 $n$，有一個質因數 $p$
Pollard's p - 1 algorithm 可以在 $p-1$ 的最大質因數很小的情況下，有效的分解合數 $n$
可以用在分解 RSA 產生的公鑰 $n$，以此破解 RSA，常見於一些 CTF 的題目中

根據費馬小定理
當 $a$ 不是 $p$ 的倍數
$a^{p-1} \equiv 1 \pmod{p} \to a^{k(p-1)} \equiv 1 \pmod{p} \to a^{k(p-1)} - 1 \equiv 0 \pmod{p}$ for some $k$
所以 $gcd(a^{k(p-1)} - 1, n)$ 一定會是 $p$ 的倍數

Pollard's p - 1 algorithm 就是嘗試去構造出 $k(p-1)$ ，並且令 $a = 2$ ( 只要 $p \ne 2$ 上面的推論就是對的 )
也就是測試 $gcd(2^{1} - 1, n), gcd(2^{1 \times 2} - 1, n), gcd(2^{1 \times 2 \times 3} - 1, n), ...$
當 $1 \times 2 \times 3 \cdots$ 是 $p-1$ 的倍數，我們就成功分解 $n$

## 使用條件

當 $p-1$ 最大的質因數很小，可以有效的分解合數 $n$

## 程式碼 ( python )

```python
import math

def pollard(n):
    a, b = 2, 2
    while True:
        a, b = pow(a, b, n), b + 1
        d = math.gcd(a - 1, n)
        if 1 < d < n:
            return d
```
