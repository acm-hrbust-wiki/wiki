```c++
//洛谷 P3381
typedef long long ll;
using namespace std;
const int INF = 1e18 + 7;
const int N = 2e4 + 7;
const int M = 2e5 + 7;
const int base = 100;
ll dis[N];
struct edge
{
    int to, nex;
    ll cap, cost;
    edge(int to = 0, int nex = 0, ll cap = 0, ll cost = 0) : to(to), nex(nex), cap(cap), cost(cost) {}
};
struct cmp
{
    bool operator()(int a, int b)
    {
        return dis[a] > dis[b];
    }
};
class MCMF // MinCostMaxFlow
{
public:
    int S, T, n, m, cnt;
    int head[N], Inq[N], pre[N];
    ll flow[N];
    edge e[M];
    void addedge(int u, int v, ll cap, ll cost)
    {
        70 e[++cnt] = edge(v, head[u], cap, cost);
        head[u] = cnt;
    }
    // 建图
    void buildGraph(int _n, int _m, int _S, int _T)
    {
        // 初始化部分
        n = _n;
        m = _m;
        S = _S;
        T = _T;
        mem(head);
        cnt = 1;
        // 构图部分
        for (int i = 1; i <= m; i++)
        {
            int u, v;
            ll cap, cost;
            read(u);
            read(v);
            readl(cap);
            readl(cost);
            addedge(u, v, cap, cost);
            addedge(v, u, 0ll, -cost);
        }
    }
    // 找增广路
    bool SPFA()
    {
        priority_queue<int, vector<int>, cmp> q;
        for (int i = 0; i <= n; i++)
        {
            dis[i] = INF;
            Inq[i] = 0;
        }
        dis[S] = 0;
        Inq[S] = 1;
        flow[S] = INF;
        // q.push(make_pair(-dis[S], S));
        q.push(S);
        int u, v;
        ll cost;
        // pair<ll, int> tmp;
        while (q.size())
        {
            u = q.top();
            q.pop();
            Inq[u] = 0;
            // u = tmp.second; cost = -tmp.first;
            for (int i = head[u]; i; i = e[i].nex)
                71
                {
                    v = e[i].to;
                    // cout << "u = " << u << " , v = " << v << "\n";
                    if (!e[i].cap)
                        continue;
                    cost = dis[u] + e[i].cost;
                    if (dis[v] > cost)
                    {
                        dis[v] = cost;
                        flow[v] = min(flow[u], e[i].cap);
                        pre[v] = i;
                        if (!Inq[v])
                        {
                            q.push(v);
                            Inq[v] = 1;
                        }
                    }
                }
        }
        return dis[T] != INF;
    }
    void update()
    {
        int u = T;
        while (u != S)
        {
            int i = pre[u];
            e[i].cap -= flow[T];
            e[i ^ 1].cap += flow[T];
            u = e[i ^ 1].to;
        }
    }
    void get_MCMF()
    {
        ll maxFlow = 0;
        ll minCost = 0ll;
        while (SPFA())
        {
            update();
            maxFlow += flow[T];
            minCost += flow[T] * dis[T];
        }
        printf("%lld %lld\n", maxFlow, minCost);
    }
    72
};
MCMF ways;
signed main()
{
    int n, m, S, T;
    while (~scanf("%d%d%d%d", &n, &m, &S, &T))
    {
        ways.buildGraph(n, m, S, T);
        ways.get_MCMF();
        // printf("%lld %lld\n", tmp.second, tmp.first);
    }
    return 0;
}
```

**例题： 网络流 24 题 餐巾计划 LOJ 6008** 

> **题意：**一个餐厅在相继的 N 天里,每天需用的餐巾数不尽相同。假设第 i 天需要 ri 块餐巾 ( i=1,2,...,N)。餐厅可以购买新的餐巾,每块餐巾的费用为 p 分;或者把旧餐巾送到快洗部,洗一 块需 m 天,其费用为 f 分;或者送到慢洗部,洗一块需 n(n>m),其费用为 s 分(s < f). 
>
> 每天结束 时,餐厅必须决定将多少块脏的餐巾送到快洗部,多少块餐巾送到慢洗部,以及多少块保存起 来延期送洗。但是每天洗好的餐巾和购买的新餐巾数之和,要满足当天的需求量。试设计一 个算法为餐厅合理地安排好 N 天中餐巾使用计划,使总的花费最小。编程找出一个最佳餐巾 使用计划。

 

**分析：**我们不妨先让每天开始时得到的 r[i]条干净的餐巾（左边一列的节点）流向 t，然后再 在每天结束时从 s 补回 r[i]条脏的餐巾（从 s 向右边一列的节点连 r[i],0 的边），得到新图的 链接方式：

```
s -> i (r,p) 每天早晨可以买最多 r 条新餐巾 一条 p 分
s -> i' (r,0) 每天用剩下 r 条脏餐巾 没有代价
i -> t (r,0) 每天要用 r 条干净餐巾 没有代价
i' -> i+m (inf,f)脏毛巾送到快洗店 洗干净送回来是第 i+m 天每条花费代价 f 分
i' ->i+n (inf,s)脏毛巾送到慢洗店 洗干净送回来是第 i+n 天 每条花费代价 s 分
i' -> (i+1)' (inf,s) 每条脏毛巾留到第二天再处理 没有代价
最后跑一次最小费用流
```

```c++
#pragma GCC optimize(2)
#include <bits/stdc++.h>
#define read(a) scanf("%d", &a)
#define readl(a) scanf("%lld", &a)
#define reads(a) scanf("%s", a)
#define readc(a) scanf("%c", &a)
#define pb push_back
#define mem(a) memset(a, 0, sizeof(a))
#define Buff ios::sync_with_stdio(false)
73 typedef long long ll;
using namespace std;
const ll INF = 1e16 + 7;
const int N = 2e5 + 7;
const int M = 1e6 + 7;
ll dis[N], flow[N];
int head[N], cnt, Inq[N], pre[N];
int S, T, n, d1, d2;
ll p, f, s;
struct edge
{
    int to, nex;
    ll w, cost;
    edge(int _to = 0, ll _w = 0, ll _cost = 0, int _nex = 0)
    {
        to = _to;
        nex = _nex;
        w = _w;
        cost = _cost;
    }
} e[M];
void addedge(int u, int v, ll w, ll cost)
{
    e[++cnt] = edge(v, w, cost, head[u]);
    head[u] = cnt;
    e[++cnt] = edge(u, 0ll, -cost, head[v]);
    head[v] = cnt;
}
queue<int> q;
bool SPAF()
{
    while (q.size())
        q.pop();
    for (int i = 0; i < N; i++)
    {
        dis[i] = INF;
        Inq[i] = 0;
    }
    q.push(S);
    dis[S] = 0;
    Inq[S] = 1;
    flow[S] = INF;
    int u, v;
    ll cost;
    while (q.size())
    {
        u = q.front();
        q.pop();
        Inq[u] = 0;
        for (int i = head[u]; i; i = e[i].nex)
        {
            if (!e[i].w)
                continue;
            v = e[i].to;
            cost = dis[u] + e[i].cost;
            if (dis[v] > cost)
                74
                {
                    dis[v] = cost;
                    flow[v] = min(flow[u], e[i].w);
                    pre[v] = i;
                    if (!Inq[v])
                    {
                        q.push(v);
                        Inq[v] = 1;
                    }
                }
        }
    }
    return dis[T] != INF;
}
ll maxFlow, minCost;
void update()
{
    int u = T, i;
    while (u != S)
    {
        i = pre[u];
        e[i].w -= flow[T];
        e[i ^ 1].w += flow[T];
        u = e[i ^ 1].to;
    }
    maxFlow += flow[T];
    minCost += flow[T] * dis[T];
}
void EK()
{
    maxFlow = minCost = 0ll;
    while (SPAF())
        update();
    printf("%lld\n", minCost);
}
void buildGraph()
{
    read(n);
    readl(p);
    read(d1);
    readl(f);
    read(d2);
    readl(s);
    mem(head);
    cnt = 1;
    S = 0;
    T = n << 1 | 1;
    for (int i = 1; i <= n; i++)
        75
        {
            ll r;
            readl(r);
            addedge(S, i, r, p);
            addedge(S, i + n, r, 0ll);
            addedge(i, T, r, 0ll);
            if (i + 1 <= n)
                addedge(i + n, i + 1 + n, INF, 0ll);
            if (i + d1 <= n)
                addedge(i + n, i + d1, INF, f);
            if (i + d2 <= n)
                addedge(i + n, i + d2, INF, s);
        }
}
signed main()
{
    buildGraph();
    EK();
    return 0;
}
```

整理人：网络 18-3 高云泽