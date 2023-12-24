```c++
int h[], e[], ne[], idx;  // 邻接表
int dfn[], low[], timestamp;  // tarjan相关数组
bool cut[];  // 点是否为割点
int rt;  // 根节点

void tarjan(int u) {  
    dfn[u] = low[u] = ++ timestamp;
    int son = 0;
    
    for (int i = h[u]; ~i; i = ne[i]) {
        int j = e[i];
        if (!dfn[j]) {
            tarjan(j);
            low[u] = min(low[u], low[j]);
            if (low[j] >= dfn[u]) {
                son ++ ;  
                if (u != rt || son > 1) cut[u] = true;
            }
        } else low[u] = min(low[u], dfn[j]);
    }
}
```

