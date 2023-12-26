#### Spfa算法

```cpp
#include <iostream>
#include <algorithm>
#include <cstring>
#include <queue>

using namespace std;

const int N = 1e5 + 10, INF = 0x3f3f3f3f;

int h[N], e[N], ne[N], w[N], idx;
int dist[N];
bool st[N];
int n, m;

void add(int a, int b, int c)
{
    w[idx] = c, e[idx] = b, ne[idx] = h[a], h[a] = idx ++ ;
}

int spfa()
{
    memset(dist, 0x3f, sizeof dist);
    dist[1] = 0;
    st[1] = true;
    
    queue<int> q;
    q.push(1);
    while (q.size())
    {
        int t = q.front();
        q.pop();
        
        st[t] = false;
        for (int i = h[t]; ~i; i = ne[i])
        {
            int j = e[i];
            if (dist[j] > w[i] + dist[t])
            {
                dist[j] = w[i] + dist[t];
                if (!st[j])
                {
                    st[j] = true;
                    q.push(j);
                }
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
    
    int t = spfa();
    
    if (t == INF) cout << "impossible" << endl;
    else cout << t << endl;
    
    return 0;
}
```

