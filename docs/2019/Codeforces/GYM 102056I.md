## 1. 题目与题目名称

POJ 1156 The Doors

## 2. 题意简述

在一个10*10大小的空间内有不超过18个墙，每个墙上有两个门。问从起点（0，5）到终点（10，5）的最短路径是多少。

## 3. 主要知识点及其运用

判断线段是否绝对相交（必须交叉到两边出头成为“X”形才认为绝对相交），根据题意建图，最短路

## 4. 完整解题思路描述

枚举每一个门的上下两个端点。将它们连线。每个门的两个端点分别抽象为图中的一个节点。建图时判断位于两个门之间的墙会不会挡住门。如果会挡住，那么认为这两个点之间的权值无穷大。如果不会挡住，那么用这两点之间的距离作为图中这条边的权值。

## 5. 技巧和坑点

枚举端点时，同一个墙上的两个门之间即使没有被挡住也不能相互联通，因为这不符合问题的定义。可以使用平面几何的知识大幅简化本题中关于线段相交的判断。

## 6. 参考文献和学习资料

http://poj.org/problem?id=1556

## 7. 完整代码

```c++
#include <cstdio>
#include <cstring>
#include <algorithm>
#include <cmath>
#include<iostream>
using namespace std;
const double INF = 1000000;
const double eps = 1e-8;
const int maxn = 1e3+3;
struct point
{
    double x,y;
    point() {}
    point(point a,point b)
    {
        this->x = b.x - a.x;
        this->y = b.y - a.y;
    }
    point(double a,double b)
    {
        x = a;
        y = b;
    }
} p[maxn];
double cross(point a,point b)
{
    return(a.x * b.y - a.y * b.x);
}
double dist(point a,point b)
{
    return sqrt((a.x - b.x)*(a.x - b.x) + (a.y - b.y)*(a.y - b.y));
}
bool judge(point a,point b,point m,point n)
{
    if( min(a.x,b.x)<=max(m.x,n.x)&&
            min(m.x,n.x)<=max(a.x,b.x)&&
            min(a.y,b.y)<=max(m.y,n.y)&&
            min(m.y,n.y)<=max(a.y,b.y)
      )
    {
        int cro1 = cross(point(a,m),point(a,b))*cross(point(a,n),point(a,b));
        int cro2 = cross(point(m,a),point(m,n))*cross(point(m,b),point(m,n));
        if(cro1<-eps&&cro2<-eps)
        {
            return 1;
        }
    }
    return 0;
}

double d[maxn][maxn];

int main()
{
    int n;
    while(scanf("%d",&n)&&n!=-1)
    {
        for(int i = 0; i<n; i++)
        {
            double x;
            scanf("%lf",&x);
            for(int j = 1; j<=4; j++)
            {
                p[i*4+j].x = x;
                scanf("%lf",&p[i*4+j].y);
            }
        }
        p[0] = point(0,5);
        p[4*n+1] = point(10,5);
        memset(d,0,sizeof(d));
        for (int i = 0 ; i <= 4 * n + 1 ; i++)
        {
            for (int j = i + 1 ; j <= 4 * n + 1 ; j++)
            {
                int l = (i - 1) / 4, r = (j - 1) / 4;
                if (i == 0)
                    l = -1;
                int flag = 1;
                if(l == r)
                {
                    d[i][j] = d[j][i] = INF;
                }
                else
                    for (int k = l + 1 ; k < r ; k++)
                    {
                        if (judge(p[4 * k + 1], point(p[4 * k + 1].x, 0), p[i], p[j]))
                        {
                            flag = 0;
                            break;
                        }
                        if (judge(p[4 * k + 2], p[4 * k + 3], p[i], p[j]))
                        {
                            flag = 0;
                            break;
                        }
                        if (judge(p[4 * k + 4], point(p[4 * k + 4].x, 10), p[i], p[j]))
                        {
                            flag = 0;
                            break;
                        }
                    }
                d[i][j] = d[j][i] = (flag ?  dist(p[i], p[j]) : INF) ;
            }
        }
        for(int i = 0; i<=4*n+1; i++)
        {
            for(int j = 0; j<=4*n+1; j++)
            {
                for(int k = 0; k<=4*n+1; k++)
                {
                    d[j][k] = min(d[j][k],d[j][i]+d[i][k]);
                }
            }
        }
        printf("%.2f\n",d[0][4*n+1]);
    }
    return 0;
}

```

## 8. 作者-江绪桢

