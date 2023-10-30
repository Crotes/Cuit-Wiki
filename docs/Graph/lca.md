[讲课链接](https://www.bilibili.com/video/BV1FP4y1v7p3/?spm_id_from=333.999.0.0&vd_source=57a1e2bae1f3574849d8f90b75cb25a2)

最近公共祖先简称 LCA（Lowest Common Ancestor）。两个节点的最近公共祖先，就是这两个点的公共祖先里面，离根最远的那个。 

## 各种算法使用场合
倍增优点：代码短，是可以维护动态加叶子的树；缺点：不能修改

树剖优点：比倍增快，支持修改；缺点：代码长

tarjan处理离线询问最快
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

```

## 倍增求LCA板子
```c++
const int N=5e5+3;
const int M=__lg(N)+1;
struct EDGE
{
    int to,nxt;
};
struct LCA
{
    EDGE e[N<<1];
    int fa[N][M],dep[N],head[N],lg[N];
    int tot;
    queue<int> q;
    void init(int n)
    {
        tot=0;
        for(int i=1;i<n;++i)
            lg[i]=lg[i-1]+(1<<lg[i-1]==i);
    }
    void AddEdge(int u,int v)
    {
        e[++tot]={u,head[v]};
        head[v]=tot;
        e[++tot]={v,head[u]};
        head[u]=tot;
    }
    void bfs(int root)
    {
        q.push(root);
        dep[root]=1;
        int u,v,DEP;
        while(!q.empty())
        {
            u=q.front();
            q.pop();
            for(int i=head[u];i;i=e[i].nxt)
            {
                v=e[i].to;
                if(dep[v])
                    continue;
                q.push(v);
                dep[v]=dep[u]+1;
                fa[v][0]=u;
                DEP=lg[dep[u]];
                for(int j=1;j<=DEP;++j)
                {
                    fa[v][j]=fa[fa[v][j-1]][j-1];
                }
            }
        }
    }
    int lca(int x,int y)
    {
        if(dep[x]>dep[y])
            swap(x,y);
        for(int i=0,dif=dep[y]-dep[x];dif;++i,dif>>=1)
        {
            if(dif&1)
                y=fa[y][i];
        }
        if(x==y)
            return x;
        for(int i=lg[dep[x]];i>=0;--i)
        {
            if(fa[x][i]==fa[y][i])
                continue;
            x=fa[x][i];
            y=fa[y][i];
        }
        return fa[x][0];
    }
};
```
## 树剖求LCA板子（有点权，有边权，无权）
### 点权
```cpp
//W和revW，原点权，按dfs序的点权
//区别于普通线段树，唯一修改build函数
//默认树以1为根
//dfs1处理重儿子，子树大小，深度，父节点
//dfs2处理重链链顶，点的dfs序
//为了减少和线段树模块的耦合度，把原点权按dfs序重排
//querySum 查询路径u-v上所有点的点权合并信息，具体和线段树有关
//update   修改路径u-v上所有点的点权，具体和线段树有关
const int N = 5e4 + 3;
ll W[N], revW[N];
struct edge
{
    int v, nxt;
};
struct SegmentTree
{
    struct node
    {
        ll sum,lz;
    }st[N<<2];
    void pushup(int id)
    {
        st[id].sum=(st[id<<1].sum+st[id<<1|1].sum);
    }
    void pushdown(int id,int lsonlen,int rsonlen)
    {
        if (!st[id].lz)
            return;
        st[id<<1].lz+=st[id].lz;
        st[id<<1|1].lz+=st[id].lz;
        st[id<<1].sum+=st[id].lz*lsonlen;
        st[id<<1|1].sum+=st[id].lz*rsonlen;
        st[id].lz=0;
    }
    void build(int id,int l,int r)
    {
        if (l ==r)
        {
            st[id]={revW[l],0};
            return;
        }
        int mid=(l + r)/2;
        build(id << 1,l,mid);
        build(id<<1|1,mid + 1,r);
        pushup(id);
    }
    void update(int id,int segl,int segr,int l,int r,ll val)
    {
        if (l <=segl && segr <=r)
        {
            st[id].sum+=val*(segr-segl+1);
            st[id].lz+=val;
            return;
        }
        int mid=(segl + segr)/2;
        pushdown(id,mid-segl+1,segr-mid);
        if (l <=mid)
            update(id << 1,segl,mid,l,r,val);
        if (r > mid)
            update(id<<1|1,mid+1,segr,l,r,val);
        pushup(id);
    }
    ll query(int id,int segl,int segr,int l,int r)
    {
        if (l <=segl && segr <=r)
            return st[id].sum;
        int mid=(segl + segr)/2;
        pushdown(id,mid-segl+1,segr-mid);
        ll res=0;
        if (l <=mid)
            res+=query(id << 1,segl,mid,l,r);
        if (r > mid)
            res+=query(id<<1|1,mid+1,segr,l,r);
        return res;
    }
};
struct heavyPathDecomposition
{
    int edgeTot, n, dfsOrder, head[N];
    edge e[N << 1];
    int son[N], siz[N], dep[N], fa[N];
    int top[N], dfn[N];
    SegmentTree st;
    void init(int nn)
    {
        n = nn;
        memset(head, 0, sizeof(int) * (n + 1));
        memset(son, -1, sizeof(int) * (n + 1));
        edgeTot = 0;
        dfsOrder = 0;
    }
    void init2()
    {
        dep[1] = 1;
        dfs1(1);
        dfs2(1, 1);
        for (int i = 1; i <= n; ++i)
            revW[dfn[i]] = W[i];
        st.build(1,1,n);
    }
    void addEdge(int u, int v)
    {
        e[++edgeTot] = {v, head[u]};
        head[u] = edgeTot;
    }
    void dfs1(int u)
    {
        siz[u] = 1;
        int v;
        for (int i = head[u]; i; i = e[i].nxt)
        {
            v = e[i].v;
            if (v == fa[u])
                continue;
            dep[v] = dep[u] + 1;
            fa[v] = u;
            dfs1(v);
            siz[u] += siz[v];
            if (son[u] == -1 || siz[v] > siz[son[u]])
                son[u] = v;
        }
    }
    void dfs2(int u, int Top)
    {
        top[u] = Top;
        dfn[u] = ++dfsOrder;
        if (son[u] == -1)
            return;
        dfs2(son[u], Top);
        for (int i = head[u]; i; i = e[i].nxt)
        {
            if (e[i].v != son[u] && e[i].v != fa[u])
                dfs2(e[i].v, e[i].v);
        }
    }
    ll querySum(int u, int v)
    {
        ll res = 0;
        while (top[u] != top[v])
        {
            if (dep[top[u]] < dep[top[v]])
                swap(u, v);
            res += st.query(1,1,n,dfn[top[u]], dfn[u]);
            u = fa[top[u]];
        }
        if (dfn[u] > dfn[v])
            swap(u, v);
        return res + st.query(1,1,n, dfn[u], dfn[v]);
    }
    void update(int u, int v, ll x)
    {
        while (top[u] != top[v])
        {
            if (dep[top[u]] < dep[top[v]])
                swap(u, v);
            st.update(1,1,n, dfn[top[u]], dfn[u], x);
            u = fa[top[u]];
        }
        if (dfn[u] > dfn[v])
            swap(u, v);
        st.update(1,1,n, dfn[u], dfn[v], x);
    }
} hp;
hp.init(n);
hp.addEdge(u,v);
hp.init2();
hp.update(u,v,x);
hp.querySum(u,v);
```

### 边权
```cpp
//把边权固定到边相连的两个点中，深度更深的那个点，就转化为点权
//dfsOrder初始化为-1，因为1为根时，它没有点权
//match是第i条边所固定的点
//只有n-1条边，所以线段树是n-1
//同一条重链上，两点之间的边不包括lca固定的那条边，所以+1，因此还有可能越界，加个判断
const int N = 1e5 + 3;
ll revW[N];
struct edge
{
    int v, nxt, id;
    ll w;
};
struct SegmentTree
{
    struct node
    {
        ll sum,lz;
    }st[N<<2];
    void pushup(int id)
    {
        st[id].sum=(st[id<<1].sum+st[id<<1|1].sum);
    }
    void pushdown(int id,int lsonlen,int rsonlen)
    {
        if (!st[id].lz)
            return;
        st[id<<1].lz+=st[id].lz;
        st[id<<1|1].lz+=st[id].lz;
        st[id<<1].sum+=st[id].lz*lsonlen;
        st[id<<1|1].sum+=st[id].lz*rsonlen;
        st[id].lz=0;
    }
    void build(int id,int l,int r)
    {
        if (l ==r)
        {
            st[id]={revW[l],0};
            return;
        }
        int mid=(l + r)/2;
        build(id << 1,l,mid);
        build(id<<1|1,mid + 1,r);
        pushup(id);
    }
    void update(int id,int segl,int segr,int l,int r,ll val)
    {
        if (l <=segl && segr <=r)
        {
            st[id].sum+=val*(segr-segl+1);
            st[id].lz+=val;
            return;
        }
        int mid=(segl + segr)/2;
        pushdown(id,mid-segl+1,segr-mid);
        if (l <=mid)
            update(id << 1,segl,mid,l,r,val);
        if (r > mid)
            update(id<<1|1,mid+1,segr,l,r,val);
        pushup(id);
    }
    ll query(int id,int segl,int segr,int l,int r)
    {
        if (l <=segl && segr <=r)
            return st[id].sum;
        int mid=(segl + segr)/2;
        pushdown(id,mid-segl+1,segr-mid);
        ll res=0;
        if (l <=mid)
            res+=query(id << 1,segl,mid,l,r);
        if (r > mid)
            res+=query(id<<1|1,mid+1,segr,l,r);
        return res;
    }
};
struct heavyPathDecomposition
{
    int edgeTot, n, dfsOrder, head[N];
    edge e[N << 1];
    int son[N], siz[N], dep[N], fa[N];
    int top[N], dfn[N];
    int match[N];
    ll W[N];
    SegmentTree st;
    void init(int nn)
    {
        n = nn;
        memset(head, 0, sizeof(int) * (n + 1));
        memset(son, -1, sizeof(int) * (n + 1));
        edgeTot = 0;
        dfsOrder = -1;
    }
    void init2()
    {
        dep[1] = 1;
        dfs1(1);
        dfs2(1, 1);
        for (int i = 2; i <= n; ++i)
            revW[dfn[i]] = W[i];
        st.build(1, 1, n - 1);
    }
    void addEdge(int u, int v, int id, ll w)
    {
        ++edgeTot;
        e[edgeTot].v = v;
        e[edgeTot].nxt = head[u];
        e[edgeTot].id = id;
        e[edgeTot].w = w;
        head[u] = edgeTot;
    }
    void dfs1(int u)
    {
        siz[u] = 1;
        int v;
        for (int i = head[u]; i; i = e[i].nxt)
        {
            v = e[i].v;
            if (v == fa[u])
                continue;
            dep[v] = dep[u] + 1;
            fa[v] = u;
            match[e[i].id] = v;
            W[v] = e[i].w;
            dfs1(v);
            siz[u] += siz[v];
            if (son[u] == -1 || siz[v] > siz[son[u]])
                son[u] = v;
        }
    }
    void dfs2(int u, int Top)
    {
        top[u] = Top;
        dfn[u] = ++dfsOrder;
        if (son[u] == -1)
            return;
        dfs2(son[u], Top);
        for (int i = head[u]; i; i = e[i].nxt)
        {
            if (e[i].v != son[u] && e[i].v != fa[u])
                dfs2(e[i].v, e[i].v);
        }
    }
    ll querySum(int u, int v)
    {
        ll res = 0;
        while (top[u] != top[v])
        {
            if (dep[top[u]] < dep[top[v]])
                swap(u, v);
            res += st.query(1,1,n, dfn[top[u]], dfn[u]);
            u = fa[top[u]];
        }
        if (dfn[u] > dfn[v])
            swap(u, v);
        if (u != v)
            res += st.query(1,1,n, dfn[u] + 1, dfn[v]);
        return res;
    }
    void update(int i, ll x)
    {
        st.update(1,1,n, dfn[match[i]], x);
    }
} hp;
hp.init(n);
hp.addEdge(u,v,i,w);
hp.init2();
hp.update(i,x);
hp.querySum(u,v);
```

### 单纯求lca
```cpp
const int N = 5e4 + 3;
struct edge
{
    int v, nxt;
};
struct heavyPathDecomposition
{
    int edgeTot,head[N];
    edge e[N<<1];
    int son[N], siz[N], dep[N], fa[N];
    int top[N];
    void init(int n)
    {
        memset(head, 0, sizeof(int) * (n + 1));
        memset(son, -1, sizeof(int) * (n + 1));
        edgeTot = 0;
    }
    void init2()
    {
        dep[1] = 1;
        dfs1(1);
        dfs2(1,1);
    }
    void addEdge(int u, int v)
    {
        e[++edgeTot] = {v, head[u]};
        head[u] = edgeTot;
    }
    void dfs1(int u)
    {
        siz[u] = 1;
        int v;
        for (int i = head[u]; i; i = e[i].nxt)
        {
            v = e[i].v;
            if (v == fa[u])
                continue;
            dep[v] = dep[u] + 1;
            fa[v] = u;
            dfs1(v);
            siz[u] += siz[v];
            if (son[u] == -1 || siz[v] > siz[son[u]])
                son[u] = v;
        }
    }
    void dfs2(int u, int Top)
    {
        top[u] = Top;
        if (son[u] == -1)
            return;
        dfs2(son[u], Top);
        for (int i = head[u]; i; i = e[i].nxt)
        {
            if (e[i].v != son[u] && e[i].v != fa[u])
                dfs2(e[i].v, e[i].v);
        }
    }
    int lca(int u,int v)
    {
        while(top[u]!=top[v])
        {
            if(dep[top[u]]<dep[top[v]])
                swap(u,v);
            u=fa[top[u]];
        }
        return (dep[u]>dep[v]?v:u);
    }
}hp;
hp.init(n);
hp.addEdge(u,v);
hp.init2();
hp.lca(u,v);
```
## 一些例题
hdu3966 点权，路径加法，单点查询

poj2763 边权，单边修改为x，路径和查询

poj3237 边权，单边修改为x，路径取相反数，路径最大值查询

loj10141 点权，路径覆盖，路径颜色段数量查询

黑暗爆炸1036 点权，单点修改，路径和，路径最大值查询

spoj qtree 边权，单边修改为x，路径最大值查询

loj1348 点权，单点修改，路径和
