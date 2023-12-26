```c++
struct Bridge {
    int u, v;
}bri[];
int h[], e[], ne[], idx;
int dfn[], low[], timestamp, cnt;  

void tarjan(int u, int from) {  
    dfn[u] = low[u] = ++ timestamp;
    
    for (int i = h[u]; ~i; i = ne[i]) {
        int j = e[i];
        if (!dfn[j]) {  
            tarjan(j);
            low[u] = min(low[u], low[j]);
            if (low[j] > dfn[u]) bri[cnt ++ ] = {u, j};
        } else if (i != (from ^ 1)) {  
            low[u] = min(low[u], dfn[j]);
        }
    }
}
```

