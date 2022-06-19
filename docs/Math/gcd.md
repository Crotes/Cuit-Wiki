## 视频

<div style = "position: relative; width: 100%; height: 0; padding-bottom: 56.25%;">
    <iframe style = "position: absolute; top: 0; left: 0; width: 100%;height: 100%;" frameborder="0" allowFullScreen="true" src="https://wiki-58c2.obs.myhuaweicloud.com:443/%E7%B4%A0%E6%95%B0%E7%AD%9B%E4%B8%8Egcd.mp4?AccessKeyId=ELA8MJ5R84QLXCTFQQ1R&Expires=1686756188&Signature=IBl5Fx8ZDUK8R9UvfIxnK%2BJGog0%3D"></iframe>
</div>

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
