// 对于单向搜索有 $2^n$ 可能的情况，若从起始点和终点同时搜，可以变成 2 个 $2^{(n/2)}$种可能，然后进行合并。例如：每次操作可以选择*a 或者+b，问数字从 n 开始操作 40 次 变成 m 的可能性有多少种。可以选择正向反向同时求出 $2^{20}$ 个答案，然后进行合并。

!!! check "整理人"
    计 16-1 刘明辉