```c++
//(一定要把 dp 方程所表示的状态设好再转移)
//hdu2089 题意：区间内不包含 62 和 4 的个数
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
const int N = 25;
int n, m;
class DigitDp
{
public:
    int dp[N][2], a[N]; ///dp[i][j]代表枚举到第 i 位前一位是 j 的方案数
    int dfs(int now, int pre, int limit)
    {
        if (!now)
            return 1;                                             ///枚举到最后一位后就要检查状态这里因为枚举的过程
        中就把状态检查完了直接返回 1 if (!limit && ~dp[now][pre]) ///如果当前位的值没达到最高且次状态已经
            访问过 return dp[now][pre];
        int up = limit ? a[now] : 9, ans = 0; ///根据 limit 是否是 1 决定下一位最多
        17 枚举到多少 for (int i = 0; i <= up; i++)
        {
            if ((pre && i == 2) || (i == 4)) ///如果前一位是 6 且当前位是 2 或者当
                前位是 4 直接跳过 continue;
            ans += dfs(now - 1, i == 6, i == up && limit); ///统计当前状态的答案
            也就是 dp 的过程
        }
        if (!limit) ///当前位值未达到最高才能记忆化
            dp[now][pre] = ans;
        return ans;
    }
    int solve(int num)
    { ///拆数字
        int cnt = 0;
        while (num)
        {
            a[++cnt] = num % 10;
            num /= 10;
        }
        memset(dp, -1, sizeof dp); ///初始化-1 的原因就是因为有些状态的方案
        数可能是 0 return dfs(cnt, 0, 1);
    }
} dp;
int main()
{
    while (scanf("%d%d", &n, &m) && n + m)
    {
        printf("%d\n", dp.solve(m) - dp.solve(n - 1));
    }
}

```

!!! check "整理人"
    网络 18-2 吴国庆