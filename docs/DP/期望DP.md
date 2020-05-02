```c++
(一般是正推)
    ///CF148D
    原来袋子里有 w 只白鼠和 b 只黑鼠 龙和王妃轮流从袋子里抓老鼠。谁先抓到白
    色老师谁就赢。 王妃每次抓一只老鼠，龙每次抓完一只老鼠之后会有一只老鼠
    跑出来。 每次抓老鼠和跑出来的老鼠都是随机的。 如果两个人都没有抓到白色
    老鼠则龙赢。王妃先抓。 问王妃赢的概率。
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
const int maxn = 1005;
const int mod = 1e9 + 7;
int w, b;
class Pdp
{
    public : double dp[maxn][maxn]; ///设 dp[i][j]表示现在轮到王妃抓时有 i 只白鼠，j 只
    黑鼠，王妃赢的概率
    void init()
    {                                ///初始化
        for (int i = 1; i <= w; i++) ///因为都是白色老鼠，抓一次肯定赢了。
            dp[i][0] = 1;
        for (int i = 0; i <= b; i++) ///因为没有白色老鼠了
            dp[0][i] = 0;
    }
    void solve()
    { ///推出递推公式
        for (int i = 1; i <= w; i++)
        {
            for (int j = 1; j <= b; j++)
            {
                if (i + j == 0)
                    continue;
                dp[i][j] += i * 1.0 / (i + j); ///白
                if (j >= 3)
                    dp[i][j] += (j * 1.0) / (i + j) * (j - 1) * 1.0 / (i + j - 1) * (j - 2) / (i + j - 2) * dp[i][j - 3]; ///黑黑黑
                if (j >= 2)
                    dp[i][j] += (j * 1.0) / (i + j) * (j - 1) * 1.0 / (i + j - 1) * (i)*1.0 /
                                (i + j - 2) * dp[i - 1][j - 2]; ///黑黑白
            }
        }
    }
    void printans()
    { ///输出答案
        printf("%.10f\n", dp[w][b]);
    }
} dp;
int main()
{
    while (~scanf("%d%d", &w, &b))
    {
        dp.init();
        dp.solve();
        dp.printans();
    }
}
```

!!! check "整理人"
    网络 18-2 吴国庆