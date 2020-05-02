```c++
/* hdu2222
题意：每组数据有 n 个模式串，询问在文本串中出现了多少模式串
*/
#include <bits/stdc++.h>
using namespace std;
const int maxn = 1e6;   // 请开成 模式串长度*模式串数量
const int ch_size = 26; // 请开成字符集大小
class Ac_automaton
{
public:
    int trie[maxn][ch_size];
    int vis[maxn], fail[maxn];
    int tot;
    void init()
    {
        memset(vis, 0, sizeof vis);
        memset(trie, 0, sizeof trie);
        tot = 0;
    }
    void insert(char *str)
    {
        int len = strlen(str);
        int pos = 0;
        for (int i = 0; i < len; i++)
        {
            int c = str[i] - 'a';
            if (!trie[pos][c])
                trie[pos][c] = ++tot;
            pos = trie[pos][c];
        }
        vis[pos]++;
    }
    void build()
    {
        queue<int> q;
        for (int i = 0; i < ch_size; i++)
        {
            78 if (trie[0][i])
            {
                fail[trie[0][i]] = 0;
                q.push(trie[0][i]);
            }
        }
        while (!q.empty())
        {
            int pos = q.front();
            q.pop();
            for (int i = 0; i < ch_size; i++)
            {
                if (trie[pos][i])
                {
                    fail[trie[pos][i]] = trie[fail[pos]][i];
                    q.push(trie[pos][i]);
                }
                else
                {
                    trie[pos][i] = trie[fail[pos]][i];
                }
            }
        }
    }
    int query(char *str)
    {
        int len = strlen(str);
        int pos = 0, ans = 0;
        for (int i = 0; i < len; i++)
        {
            int c = str[i] - 'a';
            pos = trie[pos][c];
            for (int j = pos; j && vis[j] != -1; j = fail[j])
            {
                ans += vis[j];
                vis[j] = -1;
            }
        }
        return ans;
    }
};
Ac_automaton ac;
int main()
{
    int t;
    scanf("%d", &t);
    while (t--)
    {
        ac.init();
        char str[maxn];
        int n;
        scanf("%d", &n);
        for (int i = 0; i < n; i++)
        {
            scanf("%s", str);
            79 ac.insert(str);
        }
        ac.build();
        scanf("%s", str);
        printf("%d\n", ac.query(str));
    }
}

```

整理人：计 18-5 王东琛