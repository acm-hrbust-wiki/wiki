```c++
// bfs 查找 从 s 点 能否到达 t 点
class Bfs
{
public:
    bool vis[N];
    queue<int> q;
    void bfs(int s, int t) 3
    {
        while (!q.emtpy())
            q.pop();
        memset(vis, true, sizeof(vis));
        vis[s] = false;
        q.push(s);
        while (!q.empty())
        {
            int z = q.front();
            q.pop();
            if (z == t)
                return true;
            // 搜索 z 点能够到达的所有点，入队，并且标记
        }
        return false;
    }
};

```

!!! check "整理人"
    计 16-1 刘明辉