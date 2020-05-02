```c++
//n 堆石子排成⼀列，每堆石子有一个重量 w[i]
//每次合并可以合并相邻的两堆石子，⼀次合并的代价为两堆⽯⼦的重量和 w[i]+w[i+1]。
//问安排怎样的合并顺序，能够使得总合并代价达到最大
long long w[maxn], n;
class IntervalDp
{
    public:
        long long dp[maxn][maxn], sum[maxn];
        //dp[i][j]表示把第 i 堆到第 j 堆的石子合并在⼀起的最优值
        //sum[i]为前 i 堆石子的和
        long long solve(LL *w, LL n)
        { //接口
            for (int i = 1; i <= n; i++){
                sum[i] = sum[i - 1] + w[i];
            }
            for (int len = 2; len <= n; len++){
                for (int i = 1; i <= n - len + 1; i++){
                    int j = i + len - 1;
                    for (int k = i; k <= j - 1; k++){
                        dp[i][j] = max(dp[i][j], dp[i][k] + dp[k + 1][j] + sum[j] - sum[i - 1]);
                    }
                }
            }
            return dp[1][n];
        }
};
```

!!! check "整理人"
    计18-8 鞠永全