对于一个有向图，连通分量：

对于分量中任意两点u, v，必然可以从u走到v，且从v走到u。



强连通分量：

极大连通分量。



有向图，通过缩点，转为有向无环图(DAG)(拓扑图)。



缩点：将所有联通分量缩成一个点。



求最短路/最长路：递推 $O(n+m)$



![](D:\资料\算法笔记\img\有向图强连通分量笔记1.png)



![](D:\资料\算法笔记\img\有向图强连通分量笔记2.png)



![1667999133488](D:\资料\算法笔记\img\强连通分量笔记3.png)





```cpp
int h[], e[], ne[], idx, times;  // 邻接表
int id[], scc_cnt, cnt[];  // 编号 强分数量 每个分量中的点数量
int dfn[], low[];  // tarjan相关数组
int stk[], top;  // 模拟栈
bool in_stk[];  // 是否在栈中

void tarjan(int u) {
	dfn[u] = low[u] = ++ times;
	stk[ ++ top] = u, in_stk[u] = true;
    
	for (int i = h[u]; ~i; i = ne[i]) {
		int j = e[i];
		if (!dfn[j]) {
			tarjan(j);
			low[u] = min(low[u], low[j]);
		} else if (in_stk[j]) low[u] = min(low[u], dfn[j]);
	}
	
	if (low[u] == dfn[u]) {
		scc_cnt ++ ;
		int y;
		do {
			y = stk[top -- ];
			in_stk[y] = false;
			id[y] = scc_cnt;
			cnt[scc_cnt] ++ ;
		} while (y != u);
	}
}
```

