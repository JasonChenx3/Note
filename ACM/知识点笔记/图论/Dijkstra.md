#### 朴素Dijkstra

```cpp
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

const int N = 510, INF = 0x3f3f3f3f;

int n, m;
int g[N][N];
int dist[N];
bool st[N];

int dijkstra()
{
    memset(dist, 0x3f, sizeof dist);
    dist[1] = 0;
    
    for (int i = 0; i < n; i ++ )
    {
        int t = -1;
        
        for (int j = 1; j <= n; j ++ )
            if (!st[j] && (t == -1 || dist[j] < dist[t]))
                t = j;
                
        st[t] = true;
        
        for (int j = 1; j <= n; j ++ )
            dist[j] = min(dist[j], dist[t] + g[t][j]);
    }
    
    return dist[n];
}

int main()
{
    scanf("%d%d", &n, &m);
    
    memset(g, 0x3f, sizeof g);
    
    for (int i = 0; i < m; i ++ )
    {
        int a, b, c;
        scanf("%d%d%d", &a, &b, &c);
        g[a][b] = min(c, g[a][b]);
    }
    
    int t = dijkstra();
    
    if (t == INF) t = -1;
    
    cout << t << endl;
    
    return 0;
}
```

#### 堆优化Dijkstra

```cpp
#include <iostream>
#include <algorithm>
#include <cstring>
#include <queue>

#define x first
#define y second

using namespace std;

const int N = 2e5, INF = 0x3f3f3f3f;

typedef pair<int, int> PII;

int n, m;
int h[N], e[N], ne[N], w[N], idx;
int dist[N];
bool st[N];

void add(int a, int b, int c)
{
    w[idx] = c, e[idx] = b, ne[idx] = h[a], h[a] = idx ++ ;
}

int dijkstra()
{
    priority_queue<PII, vector<PII>, greater<PII> > heap;
    memset(dist, 0x3f, sizeof dist);
    dist[1] = 0;
    heap.push({0, 1});
    
    while (heap.size())
    {
        auto t = heap.top();
        heap.pop();
        
        int id = t.y, dis = t.x;
        
        if (st[id]) continue;
        st[id] = true;
        
        for (int i = h[id]; ~i; i = ne[i])
        {
            int j = e[i];
            if (dist[j] > dis + w[i])
            {
                dist[j] = dis + w[i];
                heap.push({dist[j], j});
            }
        }
    }
    
    return dist[n];
}

int main()
{
    memset(h, -1, sizeof h);
    
    scanf("%d%d", &n, &m);
    for (int i = 0; i < m; i ++ )
    {
        int a, b, c;
        scanf("%d%d%d", &a, &b, &c);
        add(a, b, c);
    }
    
    int t = dijkstra();
    
    if (t == INF) t = -1;
    
    cout << t << endl;
    
    return 0;
}
```

