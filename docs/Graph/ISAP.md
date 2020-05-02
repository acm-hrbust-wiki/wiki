```c++
//洛谷 P3376
typedef long long ll;
using namespace std;
const int INF = 2e9 + 7;
const int N = 2e4 + 7;
const int M = 2e5 + 7;
const int base = 100;
int n, m, S, T;
struct edge
{
    int to, nex, cap;
    edge(int to = 0, int nex = 0, int cap = 0) : to(to), nex(nex), cap(cap) {}
};
class ISAP
{
public:
    int S, T, n, m, cnt;
    int head[N], d[N], cur[N], gap[N];
    edge e[M];
    void addedge(int u, int v, int cap)
    {
        e[++cnt] = edge(v, head[u], cap);
        63 head[u] = cnt;
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
        for (int i = 1, u, v, cap; i <= m; i++)
        {
            read(u);
            read(v);
            read(cap);
            addedge(u, v, cap);
            addedge(v, u, 0);
        }
    }
    // 找增广路
    void bfs()
    {
        queue<int> q;
        for (int i = 1; i <= n; i++)
        {
            cur[i] = head[i];
            d[i] = gap[i] = 0;
        }
        q.push(T);
        d[T] = gap[1] = 1;
        int u, v;
        while (q.size())
        {
            u = q.front();
            q.pop();
            for (int i = head[u]; i; i = e[i].nex)
            {
                v = e[i].to;
                if (d[v])
                    continue;
                d[v] = d[u] + 1;
                ++gap[d[v]];
                q.push(v);
            }
            64
        }
    }
    int dfs(int u, int flow)
    {
        if (u == T)
            return flow;
        int delta = 0, v, temp;
        for (int &i = cur[u]; i; i = e[i].nex)
        {
            v = e[i].to;
            if (d[v] + 1 != d[u])
                continue;
            temp = dfs(v, min(flow - delta, e[i].cap));
            if (temp)
            {
                e[i].cap -= temp;
                e[i ^ 1].cap += temp;
                delta += temp;
            }
            if (flow == delta)
                return flow;
        }
        if (!(--gap[d[u]]))
            d[S] = n + 1;
        ++gap[++d[u]];
        cur[u] = head[u];
        return delta;
    }
    int get_maxFlow()
    {
        bfs();
        int maxFlow = 0;
        while (d[S] <= n)
            maxFlow += dfs(S, INF);
        return maxFlow;
    }
};
ISAP ways;
signed main()
{
    int n, m, S, T;
    while (~scanf("%d%d%d%d", &n, &m, &S, &T))
    {
        ways.buildGraph(n, m, S, T);
        65 printf("%d\n", ways.get_maxFlow());
    }
    return 0;
}

```



**例题：**网络流 24 题 星际转移 Loj 6015 

> **题意：**现有 n 个太空站位于地球与月球之间，且有 m 艘公共交通太空船在其间来回 穿梭。每个太空站可容纳无限多的人，而每艘太空船 i 只可容纳 Hi 个人。每艘太空船将 周期性地停靠一系列的太空站，例如：1,3,4 表示该太空船将周期性地停靠太空站 134134134⋯ 每一艘太空船从一个太空站驶往任一太空站耗时均为 1。人们只能在太空船停 靠太空站（或月球、地球）时上、下船。 初始时所有人全在地球上，太空船全在初始站。 试设计一个算法，找出让所有人尽快地全部转移到月球上的运输方案。 

**分析：**这个题其实和“魔术球”这个题类似。题意就相当于一些人要从 A ---> B ，空间 站为中转点，通过飞船转移，这些飞船每一天出现在固定空间站，也即是说天数不一定的时 候，整个运输图就会发生变化，而每一天之间又是有关系的。

就好像，每一天的空间站是不 一样的（因为飞船飞行线路不一样了），所以考虑到 地球（0 号点）、n 个空间站和月亮（n + 1 号点）共 n + 2 个点，每一天分裂一次（即拥有一个新的状态），有因为共经历 t 天， 完成运输，故 将要分裂出 `t * (n + 2)` 个点【即是说将每一天的这 n + 2 个点看成一个新的 点】，枚举每一天 day，令` yesterday = (n + 2) * (day – 1)，today = (n + 2) * day；`则每一个 点 `i’ = yesterday + i`（代表昨天对应的点）,` i = today + i`（代表今天的点）；故` i’--->i`，容 量为 INF，` (n + 1) + today ---> T`，容量为 INF; 在从每一个飞船前一天所在的太空站连向后一天的太空站，流量为飞船可容纳人 数； 在最开始的时候 链接一条， `S ----> 0`，容量为 INF 的边 跑最大流直到不小于总人数即可。（每一天的最大流应累加起来）

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
typedef long long ll;
using namespace std;
const int INF = 2e9 + 7;
const int N = 1e5 + 7;
const int M = 1e6 + 7;
const int _N = 102;
const int base = 100;
int n, m, S, T;
struct edge 66
{
    int to, nex, cap;
    edge(int to = 0, int nex = 0, int cap = 0) : to(to), nex(nex), cap(cap) {}
};
int pre[_N], station[_N][_N], r[_N], H[_N];
int head[N], d[N], cur[N], gap[N];
edge e[M];
class ISAP
{
public:
    int S, T, n, m, cnt, K;
    void addedge(int u, int v, int cap)
    {
        e[++cnt] = edge(v, head[u], cap);
        head[u] = cnt;
        e[++cnt] = edge(u, head[v], 0);
        head[v] = cnt;
        // printf("%d --- %d, %d\n", u, v, cap);
    }
    // 建图
    void buildGraph()
    {
        // 初始化部分
        read(n);
        read(m);
        read(K);
        mem(head);
        cnt = 1;
        S = N - 2;
        T = N - 1;
        for (int i = 1; i <= m; i++)
        {
            read(H[i]);
            read(r[i]);
            for (int j = 1; j <= r[i]; j++)
                read(station[i][j]);
            pre[i] = 1;
        }
        n += 2;
        addedge(S, 0, INF);
        int ans = 0;
        for (int day = 1; day <= 30; day++)
            67
            {
                int today = n * day, yesterday = n * (day - 1), now;
                for (int i = 0; i < n; i++)
                    addedge(yesterday + i, today + i, INF);
                addedge(today + n - 1, T, INF);
                for (int i = 1; i <= m; i++)
                {
                    now = pre[i] + 1;
                    if (now > r[i])
                        now = 1;
                    addedge(station[i][pre[i]] + yesterday, station[i][now] + today, H[i]);
                    pre[i] = now;
                }
                ans += get_maxFlow();
                if (ans >= K)
                {
                    printf("%d\n", day);
                    return;
                }
            }
        printf("%d\n", 0);
    }
    // 找增广路
    void bfs()
    {
        queue<int> q;
        for (int i = 0; i <= T; i++)
        {
            cur[i] = head[i];
            d[i] = gap[i] = 0;
        }
        q.push(T);
        d[T] = gap[1] = 1;
        int u, v;
        while (q.size())
        {
            u = q.front();
            q.pop();
            for (int i = head[u]; i; i = e[i].nex)
            {
                v = e[i].to;
                if (d[v])
                    continue;
                68 d[v] = d[u] + 1;
                ++gap[d[v]];
                q.push(v);
            }
        }
    }
    int dfs(int u, int flow)
    {
        if (u == T)
            return flow;
        int delta = 0, v, temp;
        for (int &i = cur[u]; i; i = e[i].nex)
        {
            v = e[i].to;
            if (d[v] + 1 != d[u])
                continue;
            temp = dfs(v, min(flow - delta, e[i].cap));
            if (temp)
            {
                e[i].cap -= temp;
                e[i ^ 1].cap += temp;
                delta += temp;
            }
            if (flow == delta)
                return flow;
        }
        if (!(--gap[d[u]]))
            d[S] = T + 1;
        ++gap[++d[u]];
        cur[u] = head[u];
        return delta;
    }
    int get_maxFlow()
    {
        bfs();
        int maxFlow = 0;
        while (d[S] <= T)
            maxFlow += dfs(S, INF);
        return maxFlow;
    }
};
69 ISAP ways;
signed main()
{
    ways.buildGraph();
    return 0;
}
```

整理人：网络 18-3 高云泽