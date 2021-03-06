## Lucas定理

### 适用条件
$n$,$m$较大，$p$为素数

### 公式
$C_n^m\%p = C_{n/p}^{m/p}*C_{n\%p}^{m\%p}\%p$

### 时间复杂度
$O(f(x)+g(n)log_pn)$

$f(x)$为预处理组合数的复杂度，g(n)为单次求组合数的复杂度

$p$小，逆元打表，$O(n+p+log_p n)$

$n$为处理阶乘，$p$为打表，$log_p n$为单次查询

$p$大，$exgcd$求逆元,$O(n+ln p*log_p n)$

n为处理阶乘，$ln p*log_p n$为单次查询

### 运用
$C_{n\%p}^{m\%p}$ 的$n$和$m$小于$p$，可以直接求
```c++
long long Lucas(long long n, long long m, long long p)
{
    if (m == 0) return 1;
    return (C(n % p, m % p, p) * Lucas(n / p, m / p, p)) % p;
}
```
## exLucas

### 适用条件
$n,m$较大，$p$不为素数

### 算法流程
1. 分解$p$为质因子幂乘积的形式，即$p=\prod\limits_{i=1}^t p_i^{k_i}$
2. 对每一个$p_i^{k_i}$，求$C_n^m \% p_i^{k_i}$
3. 用CRT合并答案

怎么求$C_n^m \% p_i^{k_i}$比较复杂，要么其他人来完善，要么就当作黑盒用

求$C_n^m \% p_i^{k_i}$的原理见
[原博客](https://www.cnblogs.com/fzl194/p/9095177.html)

### 时间复杂度
$O(plog n)$

```c++
ll qpow(ll base,ll exponent,ll mod)
{
    ll result=1;
    for(;exponent;base=base*base%mod,exponent>>=1)
    {
        if(exponent&1)
            result=result*base%mod;
    }
    return result;
}
ll g(ll n,ll p)
{
    if(n<p)
        return 0;
    return g(n/p,p)+n/p;
}
ll calc(ll n,ll p,ll MOD)
{
    if(n==0)
        return 1;
    ll res=1;
    for(ll i=1;i<=MOD;i++)
    {
        if(i%p)
            res=res*i%MOD;
    }
    res=qpow(res,n/MOD,MOD);
    for(ll i=n/MOD*MOD+1;i<=n;i++)
    {
        if(i%p)
            res=res*(i%MOD)%MOD;
    }
    return res*calc(n/p,p,MOD)%MOD;
}
ll exgcd(ll a,ll b,ll &x,ll &y)
{
	if(!b) {x=1,y=0;return a;}
	ll r=exgcd(b,a%b,y,x);
	y-=x*(a/b);
	return r;
}
ll inv(ll a,ll n)
{
    ll x,y;
    if(exgcd(a,n,x,y)==1ll)
        return (x%n+n)%n;
    else
        return -1;
}
ll lucas(ll n,ll m,ll p,ll MOD)
{
    ll pw=qpow(p,g(n,p)-g(m,p)-g(n-m,p),MOD);
    return calc(n,p,MOD)*inv(calc(m,p,MOD),MOD)%MOD*inv(calc(n-m,p,MOD),MOD)%MOD*pw%MOD;
}
ll CRT(const vector<ll> &a,const vector<ll> &m)
{
    int siz=a.size();
    ll mul=1,sum=0;
    for(auto tmp:m) 
        mul*=tmp;
    for(int i=0;i<siz;i++)
    {
        ll M=mul/m[i];
        sum=(sum+a[i]*M%mul*inv(M,m[i]))%mul;
    }
    return sum%mul;
}
ll exlucas(ll n,ll m,ll MOD)
{
    vector<ll> a,mod;
    ll tmp=MOD;
    for(int p=2;p<=tmp/p;p++)
    {
        if(tmp%p==0)
        {
            ll mul=1;
            while(tmp%p==0) 
                mul*=p,tmp/=p;
            mod.push_back(mul),a.push_back(lucas(n,m,p,mul));
        }
    }
    if(tmp>1)
        mod.push_back(tmp),a.push_back(lucas(n,m,tmp,tmp));
    return CRT(a,mod);
}
```
