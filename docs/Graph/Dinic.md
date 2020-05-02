```c++
///洛谷 P3376
typedef long long ll;
using namespace std;
const int INF = 2e9 + 7;
60 const int N = 2e4 + 7;
const int M = 2e5 + 7;
const int base = 100;
struct edge
{
    int to, nex, cap;
    edge(int to = 0, int nex = 0, int cap = 0) : to(to), nex(nex), cap(cap) {}
};
class Dinic
{
public:
    int S, T, n, m, cnt;
    int head[N], d[N], cur[N];
    edge e[M];
    void addedge(int u, int v, int cap)
    {
        e[++cnt] = edge(v, head[u], cap);
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
    bool bfs()
    {
        mem(d);
        queue<int> q;
        61 q.push(S);
        d[S] = 1;
        int u, v;
        while (q.size())
        {
            u = q.front();
            q.pop();
            for (int i = head[u]; i; i = e[i].nex)
            {
                v = e[i].to;
                if (d[v] || e[i].cap <= 0)
                    continue;
                d[v] = d[u] + 1;
                q.push(v);
            }
        }
        for (int i = 0; i <= n; i++)
            cur[i] = head[i];
        return d[T];
    }
    int dfs(int u, int flow)
    {
        if (u == T)
            return flow;
        for (int &i = cur[u], v; i; i = e[i].nex)
        {
            v = e[i].to;
            if (d[v] != d[u] + 1 || e[i].cap <= 0)
                continue;
            int delta = dfs(v, min(flow, e[i].cap));
            if (delta <= 0)
                continue;
            e[i].cap -= delta;
            e[i ^ 1].cap += delta;
            return delta;
        }
        return 0;
    }
    int get_maxFlow()
    {
        int maxFlow = 0, tmp;
        while (bfs())
            while (tmp = dfs(S, INF))
                maxFlow += tmp;
        return maxFlow;
        62
    }
};
Dinic ways;
signed main()
{
    int n, m, S, T;
    while (~scanf("%d%d%d%d", &n, &m, &S, &T))
    {
        ways.buildGraph(n, m, S, T);
        printf("%d\n", ways.get_maxFlow());
    }
    return 0;
}

```

整理人：网络 18-3 高云泽