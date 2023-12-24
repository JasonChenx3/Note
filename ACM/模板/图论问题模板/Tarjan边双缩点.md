```cpp
int h[], e[], ne[], idx;
int dfn[], low[], timestamp;
int stk[], top;
int id[], dcc_cnt;
int d[];
bool is_bri[];

void tarjan(int u, int from) {
    dfn[u] = low[u] = ++ timestamp;
    stk[ ++ top] = u;
    
    for (int i = h[u]; ~i; i = ne[i]) {
        int j = e[i];
        if (!dfn[j]) {
            tarjan(j, i);
            low[u] = min(low[u], low[j]);
            if (dfn[u] < low[j]) is_bri[i] = is_bri[i ^ 1] = true;
        }
    }
    
    if (low[u] == dfn[u]) {
        ++ dcc_cnt;
        int y;
        do {
            y = stk[top -- ];
            id[y] = dcc_cnt;
        } while (y != u);
    }
}
```

