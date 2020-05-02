```c++
///求树的重心
class TreeRoot
{
public:
    int sz[maxn], f[maxn], vis[maxn], root;
    //sz[i] i 节点为根的子树大小
    // f[i] i 节点为根的最大子树大小
    //root 重心 n 树的节点数量
    void solve(int u)
    {
        sz[u] = 1;
        f[u] = 0;
        vis[u] = 1;
        for (int i = head[u]; i != -1; i = edge[i].next)
        {
            int v = edge[i].to;
            if (vis[v])
                continue;
            solve(v);
            sz[u] += sz[v];
            f[u] = max(f[u], sz[v]);
            15
        }
        f[u] = max(f[u], n - sz[u]);
        if (f[root] > f[u] || root == 0)
            root = u;
    }
    int get_root(int x)
    { //接口
        root = 0;
        memset(vis, 0, sizeof vis);
        solve(x);
        return root;
    }
};
//求树的最大独立集
vector<int> G[maxn];
class TreeNum
{
public:
    int sz[maxn], dp[maxn][2], vis[maxn];
    ///子树大小，dp[i][0/1]节点 i 选/不选的子树最优解，重复点不访问
    void dfs(int x)
    {
        vis[x] = 1;
        dp[x][0] = 0;
        dp[x][1] = 1;
        for (int i = 0; i < G[x].size(); i++)
        {
            int y = G[x][i];
            if (vis[y])
                continue;
            dfs(y);
            dp[x][0] += max(dp[y][0], dp[y][1]);
            dp[x][1] += dp[y][0];
        }
    }
    int get_num(int x)
    { //接口
        memset(vis, 0, sizeof vis);
        solve(x);
        return max(dp[x][0], dp[x][1]);
    }
};

```

!!! check "整理人"
    计18-8 鞠永全