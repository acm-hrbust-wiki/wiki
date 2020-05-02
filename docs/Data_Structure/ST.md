```c++
//洛谷 p3865 题意：查询静态区间最大值
#include <cmath>
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>
24 using namespace std;
#define LL long long
const int maxn = 1e5 + 7;
int dp[maxn][30];
int a[maxn];
class ST
{
public:
    void init_ST(int n)
    {
        memset(dp, 0, sizeof(dp));
        for (int i = 1; i <= n; i++)
        {
            dp[i][0] = a[i];
        }
        for (int j = 1; (1 << j) <= n; j++)
        {
            for (int i = 1; i + (1 << j) - 1 <= n; i++)
            {
                dp[i][j] = max(dp[i][j - 1], dp[i + (1 << (j - 1))][j - 1]);
            }
        }
    }
    int query(int l, int r)
    {
        int k = log(r - l + 1.0) / log(2.0);
        return max(dp[l][k], dp[r - (1 << k) + 1][k]);
    }
} st;
int main()
{
    int n, m;
    while (~scanf("%d%d", &n, &m))
    {
        for (int i = 1; i <= n; i++)
        {
            scanf("%d", &a[i]);
        }
        st.init_ST(n);
        while (m--)
        {
            int l, r;
            scanf("%d%d", &l, &r);
            printf("%d\n", st.query(l, r));
        }
    }
    return 0;
}
```

!!! check "整理人"
    计18-9 胡小文