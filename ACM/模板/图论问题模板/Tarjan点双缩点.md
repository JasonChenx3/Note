```cpp
int h[], e[], ne[], idx;  // 邻接表
int dfn[], low[], timestamp;  // tarjan相关数组
int stk[], top;  // 模拟栈
bool cut[];  // 标记是否为割点
int dcc_cnt;  // 点双个数，也即点双编号
vector<int> dcc[];  // 每个点双中的点
int rt;  // 当前根节点

void tarjan(int u) {
	dfn[u] = low[u] = ++ timestamp;
	stk[ ++ top] = u;
	
	if (u == rt && h[u] == -1) {
		dcc[ ++ dcc_cnt].push_back(u);
		return;
	}
	
	int cnt = 0;
	for (int i = h[u]; ~i; i = ne[i]) {
		int j = e[i];
		if (!dfn[j]) {
			tarjan(j);
			low[u] = min(low[u], low[j]);
			if (low[j] >= dfn[u]) {
				cnt ++ ;
				dcc_cnt ++ ;
                
				if (u != rt || cnt > 1) cut[u] = true;
                
				int y;
				do {
					y = stk[top -- ];
					dcc[dcc_cnt].push_back(y);
				} while (y != j);
				dcc[dcc_cnt].push_back(u);
			}
		} else low[u] = min(low[u], dfn[j]);
	}
}
```

