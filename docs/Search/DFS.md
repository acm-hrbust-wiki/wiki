题目链接：[hrbust 1743 **Word Search**](http://acm.hrbust.edu.cn/index.php?m=ProblemSet&a=showProblem&problem_id=1743)

```c++
// hrbust 1743 题意: n*m 字符矩阵，q 次查询某个字符串是否是矩阵的一条路径
#include <stdio.h>
#include <string.h>
#define N 55
char a[N][N];
char str[N];
int len;        // 存 str 字符串长度
bool vis[N][N]; //每个点只能走一次
int n, m;
class Dfs
{
public:
    int dx[10] = {0, 0, -1, 1}; // dx,dy 组成上下左右
    int dy[10] = {1, -1, 0, 0};
    bool dfs(int x, int y, int z)
    {
        if (z == len)
            return true;
        for (int i = 0; i < 4; i++)
        {
            int xx = x + dx[i];
            int yy = y + dy[i];
            if (xx > 0 && yy > 0 && xx <= n && yy <= m && a[xx][yy] == str[z] && vis[xx][yy])
            {
                vis[xx][yy] = false;
                if (dfs(xx, yy, z + 1))
                    return true;
                vis[xx][yy] = true;
            }
        }
        return false;
    }
} dfs;
int main()
{
    int t;
    scanf("%d", &t);
    while (t--)
    {
        2 int q;
        scanf("%d %d %d", &n, &m, &q);
        for (int i = 1; i <= n; i++)
            scanf("%s", a[i] + 1);
        for (int qq = 0; qq < q; qq++)
        {
            scanf("%s", str);
            len = strlen(str);
            memset(vis, true, sizeof(vis));
            int zt = 0;
            for (int i = 1; i <= n; i++)
            {
                for (int j = 1; j <= m; j++)
                {
                    vis[i][j] = false;
                    if (a[i][j] == str[0] && dfs.dfs(i, j, 1))
                    {
                        zt = 1;
                        break;
                    }
                    vis[i][j] = true;
                }
                if (zt)
                    break;
            }
            if (zt)
                printf("Yes\n");
            else
                printf("No\n");
        }
        printf("\n");
    }
}
```

!!! check "整理人"
    计 16-1 刘明辉