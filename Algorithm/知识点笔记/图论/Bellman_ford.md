#### Bellman_ford算法

```cpp
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

const int N = 510, M = 1e4 + 10, INF = 0x3f3f3f3f;

struct Edge
{
    int a, b, c;
}edges[M];

int n, m, k;
int dist[N], last[N];

void bellman_ford()
{
    memset(dist, 0x3f, sizeof dist);
    dist[1] = 0;
    
    for (int i = 0; i < k; i ++ )
    {
        memcpy(last, dist, sizeof dist);
        
        for (int j = 0; j < m; j ++ )
        {
            auto e = edges[j];
            dist[e.b] = min(dist[e.b], last[e.a] + e.c);
        }
    }
}

int main()
{
    
    scanf("%d%d%d", &n, &m, &k);
    for (int i = 0; i < m; i ++ )
    {
        int a, b, c;
        scanf("%d%d%d", &a, &b, &c);
        edges[i] = {a, b, c};
    }
    
    bellman_ford();
    
    if (dist[n] > INF / 2) cout << "impossible" << endl;
    else cout << dist[n] << endl;
    
    return 0;
}
```

