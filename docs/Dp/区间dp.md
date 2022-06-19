## 视频

<div style = "position: relative; width: 100%; height: 0; padding-bottom: 56.25%;">
    <iframe style = "position: absolute; top: 0; left: 0; width: 100%;height: 100%;" frameborder="0" allowFullScreen="true" src="https://wiki-58c2.obs.myhuaweicloud.com:443/%E5%8C%BA%E9%97%B4DP.mp4?AccessKeyId=ELA8MJ5R84QLXCTFQQ1R&Expires=1686758527&Signature=i%2B2%2Bq%2BSE35jDJmKdBge7KwVkrv0%3D"></iframe>
</div>

## 题目
[石子合并](https://vjudge.net/contest/500157#problem/L)

N堆石子摆成一条线。现要将石子有次序地合并成一堆。规定每次只能选相邻的2堆石子合并成新的一堆，并将新的一堆石子数记为该次合并的代价。计算将N堆石子合并成一堆的最小代价。

例如： 1 2 3 4，有不少合并方法

1 2 3 4 => 3 3 4(3) => 6 4(9) => 10(19)

1 2 3 4 => 1 5 4(5) => 1 9(14) => 10(24)

1 2 3 4 => 1 2 7(7) => 3 7(10) => 10(20)

括号里面为总代价可以看出，第一种方法的代价最低，现在给出n堆石子的数量，计算最小合并代价。

```c++
#include<cstdio>
#include<iostream>
#include<algorithm>
#define INF 0x3f3f3f3f
using namespace std;
int M[105],dp[105][105],Add[105];
int main(void){
	int n;
	scanf("%d",&n);
	for(int i=1;i<=n;i++)
		scanf("%d",&M[i]);
	for(int i=1;i<=n;i++)
		Add[i]=Add[i-1]+M[i];//Add表示前缀和，记录前i个石子数量
	for(int i=1;i<=n;i++)
		for(int j=2;j<=n;j++)
			dp[i][j]=INF;//确保能获得最小值，长度为1的各段初始花费为0 
	for(int j=1;j<=n;j++)//枚举长度，从1到n的长度
		for(int i=1;i+j-1<=n;i++)//枚举起点，确保长度不超过总长度 
			for(int k=1;k<j;k++)//枚举分割点，从1到j-1 
				dp[i][j]=min(dp[i][j],dp[i][k]+dp[i+k][j-k]+Add[i+j-1]-Add[i-1]);
	printf("%d",dp[1][n]);
} 
```

