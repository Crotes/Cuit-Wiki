## 最大公约数和最小公倍数 

两个数$a$和$b$的最大公约数(Greatest Common Divisor)是指  
同时整除$a$和$b$的最大因数，记为$gcd(a, b)$  
当$gcd(a, b) = 1$，我们称$a$和$b$互素(又称互质)    
两个数$a$和$b$的最小公倍数(Leatest Common Multiple)是指  
同时被$a$和$b$整除的最小倍数，记为$lcm(a, b)$  
$gcd$是基础数论中非常重要的概念，求解$gcd$一般采用欧几里得算法  
而求$lcm$通过$lcm(a, b) = ab / gcd(a, b)$求解  
$gcd$有1个性质：$gcd(a,b)=gcd(b,a\%b)$  
为了求解，人为规定$gcd(a,0)=a$  
```
/*
欧几里得算法（又名辗转相除法）
时间复杂度(log n)
*/
//手写
int gcd(int a,int b)
{
	return b ? gcd(b,a%b) : a;
}
//库函数,最好用这个
//<algorithm> 里的__gcd
```
