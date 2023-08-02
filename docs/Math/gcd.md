[讲课链接](https://www.bilibili.com/video/BV1hP411F7gq/?spm_id_from=333.999.0.0)

两个正整数 $a,b$ 的最大公约数(Greatest Common Divisor)是指同时整除 $a,b$的最大因数，记为$gcd(a, b)$

当 $gcd(a, b) = 1$，我们称 $a$ 和 $b$ 互素(又称互质)

两个正整数 $a,b$ 的最小公倍数(Leatest Common Multiple)是指同时被 $a,b$ 整除的最小倍数，记为$lcm(a, b)$  

显然二者有如下关系： $lcm(a, b) = \frac{ab}{gcd(a, b)}$ 
## 辗转相减法&辗转相除法
$gcd(a,b)=gcd(a,b-a)$

$gcd(a,b)=gcd(a,b%a)$

求解 $gcd$ 一般采用欧几里得算法（又名辗转相除法）,时间复杂度 $O(\log n)$

为了方便求解，人为规定$gcd(a,0)=a$
### 辗转相减推式子例题
设 $j>i$

$gcd(2^i-1,2^j-1)=gcd(2^i-1,2^j-2^i)=gcd(2^i-1,2^i(2^{j-i}-1))$

因为 $gcd(2^i-1,2^i)=1$

所以 $gcd(2^i-1,2^i(2^{j-i}-1))=gcd(2^i-1,2^{j-i}-1)$

发现指数上的辗转相减形式

$gcd(2^i-1,2^j-1)=2^{gcd(i,j)}-1$
## gcd相关函数
在Cpp11，GNU引入了__gcd()，在Cpp17，Std引入了gcd()，二者都在algorithm库

GNU是Std超集，如果你使用的编译器是g++，且版本至少Cpp11，那么你可以使用__gcd()

如果你的编译器版本至少Cpp17，那么你可以使用gcd()
```
int gcd(int a,int b)//手写版本
{
	return b ? gcd(b,a%b) : a;
}
```
