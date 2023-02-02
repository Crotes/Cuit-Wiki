[讲课链接](https://www.bilibili.com/video/BV1hP411F7gq/?spm_id_from=333.999.0.0)

## 最大公约数(Greatest Common Divisor)和最小公倍数(Leatest Common Multiple)

两个数$a$和$b$的最大公约数是指同时整除$a$和$b$的最大因数，记为$gcd(a, b)$

当$gcd(a, b) = 1$，我们称$a$和$b$互素(又称互质)    

两个数$a$和$b$的最小公倍数是指同时被$a$和$b$整除的最小倍数，记为$lcm(a, b)$  

求解$gcd$一般采用欧几里得算法（又名辗转相除法）,时间复杂度 $O(\log n)$

而求$lcm$通过$lcm(a, b) = \frac{ab}{gcd(a, b)}$求解

根据欧几里得算法：$gcd(a,b)=gcd(b,a\%b)$

为了方便求解，人为规定$gcd(a,0)=a$

在Cpp11，GNU引入了__gcd()，在Cpp17，Std引入了gcd()，二者都在algorithm库

GNU是Std超集，如果你使用的编译器是g++，且版本至少Cpp11，那么你可以使用__gcd()

如果你的编译器版本至少Cpp17，那么你可以使用gcd()
```
int gcd(int a,int b)//手写版本
{
	return b ? gcd(b,a%b) : a;
}
```
