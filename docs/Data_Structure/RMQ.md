```c++
//poj3264 静态区间最大值减最小值
#include <map>
#include <cmath>
#include <vector>
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>
using namespace std;
#define LL long long
const int maxn = 1e5 + 7;
const int mod = 99991;
const int INF = 0x3f3f3f3f;
int MIN[maxn << 2], MAX[maxn << 2];
class RMQ
{
public:
#define lson l, m, rt << 1
#define rson m + 1, r, rt << 1 | 1
    void push_up(int rt)
    {
        MIN[rt] = min(MIN[rt << 1], MIN[rt << 1 | 1]);
        MAX[rt] = max(MAX[rt << 1], MAX[rt << 1 | 1]);
    }
    void build(int l, int r, int rt)
    {
        if (l == r)
        {
            scanf("%d", &MIN[rt]);
            MAX[rt] = MIN[rt];
            return;
        }
        int m = (l + r) >> 1;
        build(lson);
        build(rson);
        push_up(rt);
    }
    int query_min(int L, int R, int l, int r, int rt)
    {
        26 if (L <= l && r <= R)
        {
            return MIN[rt];
        }
        int IMIN = INF;
        int m = (l + r) >> 1;
        if (L <= m)
            IMIN = min(IMIN, query_min(L, R, lson));
        if (R > m)
            IMIN = min(IMIN, query_min(L, R, rson));
        return IMIN;
    }
    int query_max(int L, int R, int l, int r, int rt)
    {
        if (L <= l && r <= R)
        {
            return MAX[rt];
        }
        int IMAX = 0;
        int m = (l + r) >> 1;
        if (L <= m)
            IMAX = max(IMAX, query_max(L, R, lson));
        if (R > m)
            IMAX = max(IMAX, query_max(L, R, rson));
        return IMAX;
    }
} rmq;
int main()
{
    int n, q;
    while (~scanf("%d%d", &n, &q))
    {
        rmq.build(1, n, 1);
        while (q--)
        {
            int l, r;
            scanf("%d%d", &l, &r);
            printf("%d\n", rmq.query_max(l, r, 1, n, 1) - rmq.query_min(l, r, 1, n, 1));
        }
    }
    return 0;
}
```

> 注：这是基于线段树实现 RMQ，它支持动态 RMQ 问题的求解，至于区间修改的代码，请详 见线段树模板。

整理人：计 18-9 胡小文