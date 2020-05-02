## 1. 题目与题目名称

CodeForces 505D Mr. Kitayuta’s Technology

## 2. 题意简述

给n个点，然后给出m个条件a、b，表示a可到达b，求最少需要建多少条有向边才能满足所有条件。


## 3. 主要知识点及其运用

构造、拓扑、bfs

## 4. 完整解题思路描述

首先对于每个单独的连通块（把边都看成双向边），我们可以把它拉成一条链或一个环。

链的情况如样例一：1->2->3->4。这样前面的点都可以到达相对位置后面的点，且总边数最小，为点数-1。但是若该联通块中存在环，则无法拉成一根链，如样例二，但是我们可以通过加一条链尾到链首的边解决所有的环的冲突。在样例二中为1->2->3->4->1。这样总边数最小，为点数。

所以先把边看成双向边，对每个连通块染色，然后拓扑判断是否存在环，若存在，答案+=连通块中的点数；否则+= 连通块中的点数-1。

## 5. 技巧和坑点

需要想到链和环分别应该如何构造。

## 6. 参考文献和学习资料

http://codeforces.com/contest/505/problem/D

## 7. 完整代码

```c++
#include <bits//stdc++.h>
using namespace std;
struct EDGE{
    int to,next;
}edge[500005],dedge[500005];
int head[100005],dhead[100005],cnt,dcnt;
void addedge(int fr,int to,int k){
    if (!k){
        edge[cnt]={to,head[fr]};
        head[fr]=cnt++;
    }
    else{
        dedge[dcnt]={to,dhead[fr]};
        dhead[fr]=dcnt++;
        dedge[dcnt]={fr,dhead[to]};
        dhead[to]=dcnt++;
    }
}
int ans,color[100005],n,m,in[100005];
void init(){
    memset(color,-1,sizeof(color));
    memset(head,-1,sizeof(head));
    memset(dhead,-1,sizeof(dhead));
    cnt=dcnt=0;
}
queue<int> q2;
vector<int> pp;
void bfs(int q){
    while (q2.size()) q2.pop();
    pp.clear();
    queue<int> que;
    que.push(q);
    while (que.size()){
        int k=que.front();
        que.pop();
        pp.push_back(k);
        if (!in[k]) q2.push(k);
        for (int i=dhead[k];i!=-1;i=dedge[i].next){
            int v=dedge[i].to;
            if (color[v]!=-1) continue;
            color[v]=color[k];
            que.push(v);
        }
    }
}
void tuopu(int q){
    if (pp.size()<=1) return;
    while (q2.size()){
        int k=q2.front();
        q2.pop();
        for (int i=head[k];i!=-1;i=edge[i].next){
            int v=edge[i].to;
            in[v]--;
            if (!in[v]) q2.push(v);
        }
    }
    bool key=0;
    for (int i=0;i<pp.size();i++){
        if (in[pp[i]]){
            key=1;
            break;
        }
    }
    if (key) ans+=pp.size();
    else ans+=pp.size()-1;
}

int main(){
    init();
    cin>>n>>m;
    for (int i=0;i<m;i++){
        int u,v;
        cin>>u>>v;
        addedge(u,v,0);
        addedge(u,v,1);
        in[v]++;
    }
    int num_color=0;
    for (int i=1;i<=n;i++){
        if (color[i]==-1){
            color[i]=num_color++;
            bfs(i);
            tuopu(color[i]);
        }
    }
    cout<<ans<<endl;
}

```

## 8. 作者-顾佳昊

