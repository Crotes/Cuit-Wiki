## 视频

<div style = "position: relative; width: 100%; height: 0; padding-bottom: 56.25%;">
    <iframe style = "position: absolute; top: 0; left: 0; width: 100%;height: 100%;" frameborder="0" allowFullScreen="true" src="https://wiki-58c2.obs.myhuaweicloud.com:443/lca.mp4?AccessKeyId=ELA8MJ5R84QLXCTFQQ1R&Expires=1688147512&Signature=ZQYCgT0xUBYnxFtky629s3wkRY8%3D"></iframe>
</div>

## 题目

```c++
#include<iostream>
#include<cstdio>
using namespace std;
const int maxn = 5e5 + 5;
struct edge{
    int to;
    int next;
}edge[maxn << 1];
int head[maxn], cnt;
void add(int x, int y){
    edge[++cnt].to = y;
    edge[cnt].next = head[x];
    head[x] = cnt;
}
int depth[maxn], fa[maxn][21];
int lg[maxn];
void dfs(int now, int fath){
    fa[now][0] = fath; // 直接父亲
    depth[now] = depth[fath] + 1;
    for(int i = 1;i < lg[depth[now]];i++){ // 10011011 fa[now][8] fa[now][9 - 20]
        fa[now][i] = fa[fa[now][i-1]][i-1];
    }   
    for(int i = head[now];i;i = edge[i].next){
        if(edge[i].to != fath) dfs(edge[i].to, now);
    }
}
int lca(int x, int y){
    if(depth[x] < depth[y]) swap(x, y);//令dep[x] > dep[y] 
    while(depth[x] > depth[y]){ // 统一深度
        x = fa[x][lg[depth[x] - depth[y]] - 1];// 10011011 00011011 00001011 00000011 00000001 00000000
    }
    if(x == y) return x; // 如果相遇了就是最近公告祖先
    for(int k = lg[depth[x] - 1];k >= 0;k--){ // 先跨大步再跨小步 10011011  7 8
        if(fa[x][k] != fa[y][k]){ // !!!
            x = fa[x][k];
            y = fa[y][k];
        }
    }
    return fa[x][0]; // 最后再跳一步
}
int main(void){
    for(int i = 1;i <= 10;i++){
        lg[i] = lg[i-1] + (1 << lg[i-1] == i);
    }
    int n, m, s;
    scanf("%d %d %d",&n, &m, &s);
    for(int i = 1;i < n;i++){
        int x, y;
        scanf("%d %d",&x, &y);
        add(x, y);
        add(y, x);
    }
    
    dfs(s,0); // 预处理
    for(int i = 1;i <= m;i++){
        int x, y;
        scanf("%d %d",&x, &y);
        printf("%d\n",lca(x, y));
    }
    return 0;
}

// 倍增lca  tarjan的lca
```

