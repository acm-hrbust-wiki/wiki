```c++
///洛谷 P3376
typedef long long ll;
using namespace std;
const int INF = 2e9 + 7;
const int N = 2e4 + 7;
const int M = 2e5 + 7;
struct edge
{
    int to, nex, cap;
    edge(int to = 0, int nex = 0, int cap = 0) : to(to), nex(nex), cap(cap) {}
};
class EK
{
public:
    int S, T, n, m, cnt;
    int head[N], vis[N];
    int pre[N], flow[N];
    edge e[M];
    void addedge(int u, int v, int cap) 58
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
        mem(vis);
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
        mem(vis);
        queue<int> q;
        q.push(S);
        vis[S] = 1;
        flow[S] = INF;
        int u, v;
        while (q.size())
        {
            u = q.front();
            q.pop();
            for (int i = head[u]; i; i = e[i].nex)
            {
                v = e[i].to;
                if (vis[v] || e[i].cap <= 0)
                    continue;
                flow[v] = min(flow[u], e[i].cap);
                pre[v] = i;
                q.push(v);
                vis[v] = 1;
                if (v == T)
                    return 1;
            }
        }
        return 0;
        59
    }
    int update()
    {
        int u = T;
        while (u != S)
        {
            int i = pre[u];
            e[i].cap -= flow[T];
            e[i ^ 1].cap += flow[T];
            u = e[i ^ 1].to;
        }
        return flow[T];
    }
    int get_maxFlow()
    {
        int maxFlow = 0;
        while (bfs())
            maxFlow += update();
        return maxFlow;
    }
};
EK ways;
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