```c++
// (一般是逆推)19
//     ///POJ2096 一个软件有 s 个子系统，会产生 n 种 bug
//     某人一天发现一个 bug,
//     这个 bug 属于一个子系统，属于一个分类
//         每个 bug 属于某个子系统的概率是 1 /
//         s,
//     属于某种分类的概率是 1 / n
//         问发现 n 种 bug,
//     每个子系统都发现 bug 的天数的期望。 
const int N = 1005;
const int mod = 1e9 + 7;
int n, m;
class Edp
{
public:
    double dp[N][N]; ///dp[i][j]表示已经找到 i 种 bug,j 个系统的 bug，达到目标状
    态的天数的期望

    void init()
    {                 ///初始化
        dp[n][m] = 0; ///dp[n][m]=0;
    }
    void solve()
    { ///推导出递推方程，每一种状态的概率不要算错(要用 double)
        for (int i = n; i >= 0; i--)
        {
            for (int j = m; j >= 0; j--)
            {
                if (i == n && j == m)
                    continue;
                double p1 = (i * j) * 1.0 / (n * m);
                double p2 = ((n - i) * j) * 1.0 / (n * m);
                double p3 = (i * (m - j)) * 1.0 / (n * m);
                double p4 = ((n - i) * (m - j)) * 1.0 / (n * m);
                dp[i][j] = (dp[i + 1][j] * p2 + dp[i][j + 1] * p3 + dp[i + 1][j + 1] * p4 + 1.0) / (1 - p1);
            }
        }
    }
    void printans()
    {                               ///输出
        printf("%.4f\n", dp[0][0]); ///要求的答案
    }
} dp;
```

!!! check "整理人"
    网络 18-2 吴国庆