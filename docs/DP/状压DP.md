```c++
///hdu5418 题意：n 个城市，m 条无向边，问从 1 号城市经过所有城市在回到 1 号城市的
最少花费
#include <bits/stdc++.h>
using namespace std;
const int maxn = 2e6 + 6;
const int N = 20; ///城市点数
const int inf = 0x3f3f3f3f;
const int mod = 1e9 + 7;
int n, m;    ///城市数量，边数量
int a[N][N]; ///邻接矩阵存图
class State
{
public:
    int dp[N][(1 << N) + 10]; ///dp[i][S] 为当前在 i 走过的集合为 S 的花费,
    int solve()
    {
        for (int k = 0; k < n; k++)
        {
            for (int i = 0; i < n; i++)
            {
                for (int j = 0; j < n; j++)
                {
                    a[i][j] = min(a[i][j], a[i][k] + a[k][j]);
                }
            }
        } ///floyd 求两点间最短距离
        memset(dp, inf, sizeof dp);

        dp[0][1] = 0;
        int limit = 1 << n;
        for (int S = 0; S < limit; S++)
        {
            for (int i = 0; i < n; i++)
                if ((1 << i) & S)
                { ///起点
                    for (int j = 0; j < n; j++)
                        if (!((1 << j) & S))
                        { ///终点
                            dp[j][S | (1 << j)] = min(dp[j][S | (1 << j)], dp[i][S] + a[i][j]);
                        }
                }
        }
        int ans = inf;
        14 for (int i = 0; i < n; i++)
        {
            ans = min(ans, dp[i][limit - 1] + a[0][i]);
        }
        return ans;
    }
} zdp;
int main()
{
    int t;
    scanf("%d", &t);
    while (t--)
    {
        scanf("%d%d", &n, &m);
        for (int i = 0; i < n; i++)
        {
            for (int j = 0; j < n; j++)
                a[i][j] = (i == j ? 0 : inf);
        }
        for (int i = 1; i <= m; i++)
        {
            int u, v, w;
            scanf("%d%d%d", &u, &v, &w);
            u--;
            v--;                       ///边从 0 开始计数，方便转移
            a[u][v] = min(a[u][v], w); ///避免有重边
            a[v][u] = a[u][v];
        }
        printf("%d\n", zdp.solve());
    }
}
```

!!! check "整理人"
    计18-8 鞠永全