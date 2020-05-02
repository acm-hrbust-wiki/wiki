///HDU2255 求最优匹配 题意：有 N 家老百姓和 N 间房子，要给每家分配一间房，每个村 民对不同的房子出价不同，现在村长要让利益最大化，问应怎么分配才能让村民出的钱数总 和最大。

```c++
#include <cstdio>
#include <cstring>
#include <algorithm>
using namespace std;
int mp[303][303]; //记录村民对各个房子出价情况
int lin[303];     //标记房子所匹配到的村民
int viy[303];     //标记房子是否被访问过
int vix[303];     //标记村民是否被访问过
int x[303];       //记录村民的点权
int y[303];       //记录房子的点权
int n;
int lack, t; //lack 记录单次要改变的点权大小
int dfs(int u)
{
    vix[u] = 1;
    for (int i = 1; i <= n; i++)
    {
        if (!viy[i])
        {
            56 t = x[u] + y[i] - mp[u][i];
            if (t == 0) //t 为零代表 u 村民可以和 i 房子匹配
            {
                viy[i] = 1;
                if (dfs(lin[i]) || !lin[i]) //i房子没匹配过或i房子匹配到的村民可以与其它房
                    子匹配
                    {
                        lin[i] = u;
                        return 1;
                    }
            }
            else if (lack > t)
                lack = t;
        }
    }
    return 0;
}
void KM()
{
    memset(y, 0, sizeof(y));
    memset(x, 0, sizeof(x));
    memset(lin, 0, sizeof(lin));
    for (int i = 1; i <= n; i++)
        for (int j = 1; j <= n; j++)
            x[i] = max(x[i], mp[i][j]); //x[i]记录 i 村民所有出价中最大的值
    for (int i = 1; i <= n; i++)
    {
        while (1)
        {
            memset(vix, 0, sizeof(vix));
            memset(viy, 0, sizeof(viy));
            lack = 1e9 + 7;
            if (dfs(i)) //i 村民匹配成功
                break;
            for (int i = 1; i <= n; i++)
            {
                if (vix[i])
                    x[i] -= lack; //村民点权减小
                if (viy[i])
                    y[i] += lack; //房子点权增大
            }
        }
    }
}
int main()
{
    57 while (~scanf("%d", &n))
    {
        memset(mp, 0, sizeof(mp));
        for (int i = 1; i <= n; i++)
            for (int j = 1; j <= n; j++)
            {
                scanf("%d", &mp[i][j]);
            }
        KM();
        int ans = 0;
        for (int i = 1; i <= n; i++)
        {
            ans += mp[lin[i]][i]; //求和
        }
        printf("%d\n", ans);
    }
}

```

整理人：计 18-5 王佳妮