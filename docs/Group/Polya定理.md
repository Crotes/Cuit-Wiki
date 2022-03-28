# Polya定理

设 $G=\{π_1,π_2,π_3,...,π_k\}$是 $X=\{a_1,a_2,a_3,...,a_n\}$ 上一个置换群，

//置换的集合即为置换群

用 $m$ 种颜色对 $X$ 中的元素进行涂色，那么不同的涂色方案数为：

$\frac{1}{|G|} * \sum\limits_{i=1}^{k} m^{C(π_i)}$

---
## 例题
黑白两种颜色对下面的2*2方格进行染色, 

如果允许方格可以绕中心点旋转, 问有多少种不同的着色方案数?

$\begin{matrix}1&2\\3&4\end{matrix}$

---

方格可以旋转 $0°,90°,180°,270°$.

所以群 $G = \{0°,90°,180°,270°\}$ ,$|G| = 4$

$G$ 中 所有的置换是

$π_1 = (1)(2)(3)(4)$

$π_2 = (1234)$

$π_3 = (14)(23)$

$π_4 = (1432)$

$c(π_1) = 4 , c(π_2) = 1  , c(π_3) = 2 , c(π_4) = 1$

带入 Polya定理

$ans = \frac{1}{4}*(2^4+2^1+2^2+2^1) = 6$

---

## 原理
当只有旋转的时候(顺时针或逆时针)，

对于一个有 $n$ 个元素的环，可顺时针或逆时针旋转几个位置，

由于至少有 $n$ 个置换，

但是假设我顺时针旋转 $k$ 个位置，他就等同于逆时针转动 $n-k$ 个位置，

假设一个置换为:

$G=\{π_0,π_1,π_2,...,π_{n-1}\}$

这个时候可以证明

逆时针旋转 $k$ 个位置时 

$π_k$ 的循环节数为 $gcd(n,k)$

且每个循环的长度为 $L=n/gcd(n,k)$

---

上面那个例题只考虑了旋转的情况

## 翻转
+ 当 $n$ 为奇数的时候，只有一种形式，

以顶点 $i$ 与中心的连线为轴的翻转

$π_i=\begin{matrix}...&i-1&i&i+1&...\\...&i+1&i&i-1&...\end{matrix}$

有 $n$ 个循环节数为 $(n+1)/2$ 的置换

+ 当n为偶数时，有两种形式：

1. 以顶点 $i$ 与中心的连线为轴的翻转

$π_i=\begin{matrix}...&i-1&i&i+1&...\\...&i+1&i&i-1&...\end{matrix}$

有 $n/2$ 个循环节数为 $n/2+1$ 的置换

2. 以顶点 $i$ 和 $i+1$ 的中点与中心的连线为轴翻转

$π_i=\begin{matrix}...&i-1&i&i+1&...\\...&i+2&i+1&i&...\end{matrix}$

有 $n/2$ 个循环节数为 $n/2$ 的置换

>要特别注意 $0$ 的情况，输出 $0$ 即可。  

由于旋转有 $n$ 种置换，翻转也有 $n$ 种置换，所以 $|G|=2n$

---

## 例题
rotate 旋转

reflection to the axis of symmetry 翻转

poj 1286 考虑旋转和翻转

poj 2409 考虑旋转和翻转

poj 2154 考虑旋转

```c++
typedef long long ll;
ll fast_power(ll base,ll exponent)
{
    ll result=1;
    while(exponent>0)
    {
        if(exponent&1)
            result=result*base;
        base=base*base;
        exponent>>=1;
    }
    return result;
}
ll polya(ll n,ll m)
{
    if(!n)
        return 0;
    ll sum=0;
    for(ll i=0;i<n;++i)
        sum+=fast_power(m,__gcd(n,i));
    if(n&1)
        sum+=n*fast_power(m,(n+1)/2);
    else
        sum+=n/2*(1ll+m)*fast_power(m,n/2);
    sum/=(2*n);
    return sum;
}
```
