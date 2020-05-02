```c++
// 暴力搜索，但对于某些特定情况可以通过 alphat-beta 值直接优化掉
#include <stdio.h>
#include <math.h>
#include <algorithm>
using namespace std;
char a[10][10];
4 class MinimaxSearch
{
public:
    int xx, yy;
    int Evaluate() //判断 1 是 x 胜利 -1 是 o 胜利 0 是没有胜利
    {
        int i, j, z, zz, zt, zzt;
        for (i = 0; i < 4; i++)
        {
            z = 0, zz = 0, zt = 0, zzt = 0;
            for (j = 0; j < 4; j++)
            {
                if (a[i][j] == 'x')
                    z++;
                else if (a[i][j] == 'o')
                    zz++;
                if (a[j][i] == 'x')
                    zt++;
                else if (a[j][i] == 'o')
                    zzt++;
            }
            if (z == 4 || zt == 4)
                return 1;
            if (zz == 4 || zzt == 4)
                return -1;
        }
        zt = 0, zzt = 0, z = 0, zz = 0;
        for (i = 0; i < 4; i++)
        {
            if (a[i][i] == 'x')
                z++;
            if (a[i][i] == 'o')
                zz++;
            if (a[i][3 - i] == 'x')
                zt++;
            if (a[i][3 - i] == 'o')
                zzt++;
        }
        if (z == 4 || zt == 4)
            return 1;
        if (zz == 4 || zzt == 4)
            return -1;
        return 0;
    }
    int Max(int depth, int upalpha, int upbeta)
    {
        5 int alpha = upalpha, beta = upbeta;
        int val;
        int flag = Evaluate();
        if (flag || depth == 0) //已经有一方胜利，或者全下完
            return flag;
        int i, j;
        for (i = 0; i < 4; i++)
        {
            for (j = 0; j < 4; j++)
            {
                if (a[i][j] == '.')
                {
                    a[i][j] = 'x';
                    val = Min(depth - 1, alpha, beta);
                    a[i][j] = '.';
                    alpha = max(alpha, val);
                    if (alpha >= beta)
                    {
                        xx = i, yy = j;
                        return alpha;
                    }
                }
            }
        }
        return alpha;
    }
    int Min(int depth, int upalpha, int upbeta)
    {
        int alpha = upalpha, beta = upbeta;
        int val;
        int flag = Evaluate();
        if (flag || depth == 0) //已经有一方胜利，或者全下完
            return flag;
        int i, j;
        for (i = 0; i < 4; i++)
        {
            for (j = 0; j < 4; j++)
            {
                if (a[i][j] == '.')
                {
                    a[i][j] = 'o';
                    val = Max(depth - 1, alpha, beta);
                    a[i][j] = '.';
                    beta = min(beta, val);
                    if (beta <= alpha)
                        return beta;
                    6
                }
            }
        }
        return beta;
    }
} miniMax;
int main()
{
    char c;
    while (scanf("%c", &c) != EOF)
    {
        if (c == '$')
            break;
        int i, j;
        int sum = 0;
        for (i = 0; i < 4; i++)
        {
            scanf("%s", a[i]);
            for (j = 0; j < 4; j++)
            {
                if (a[i][j] == '.')
                    sum++;
            }
        }
        if (sum > 11) //刚下四个子时不可能有决胜点，没有这个一直 TLE
        {
            printf("#####\n");
            getchar();
            continue;
        }
        int z = miniMax.Max(sum, -1, 1);
        if (z == 1)
            printf("(%d,%d)\n", miniMax.xx, miniMax.yy);
        else
            printf("#####\n");
        getchar();
    }
}

```

!!! check "整理人"
    计 16-1 刘明辉