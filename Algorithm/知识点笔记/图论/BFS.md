### 介绍

#### BFS

- 求最短距离
- 求最小步数

#### BFS的特点

- 两段性与单调性

- “求最小”
- 基于迭代，不会爆栈

#### 四联通与八联通

- 四联通是有==重复的边==即为联通。

- ```c++
  // 四联通一般写法(不含自身)
  int dx[4] = {1, 0, -1, 0}, dy[4] = {0, 1, 0, -1};
  
  for (int i = 0; i < 4; i ++ )
  {
      int xx = x + dx[i];
      int yy = y + dy[i];
  }
  
  // 四联通一般写法(含自身)
  int dx[4] = {0, 1, 0, -1, 0}, dy[4] = {0, 0, 1, 0, -1};
  
  for (int i = 0; i < 5; i ++ )
  {
      int xx = x + dx[i];
      int yy = y + dy[i];
  }
  ```



- 八联通是有==重复的点==即为联通

- ```c++
  // 八联通一般写法
  for (int i = x - 1; i <= x + 1; i ++ )
      for (int j = y - 1l j <= y + 1; j ++ )
      {
          if (i == x && j == y) continue;
      }
  ```

#### 正确性证明

- 证明==两段性==与==单调性== 。



### 算法

#### Flood Fill算法

- 可以在线性复杂度内，找到某个点所在的连通块。

#### 最短路模型

- 所有边的权重都相等，第一次搜到时，这条路径就是最短路。

#### 多源BFS

- 建立虚拟原点，虚拟原点到起点的距离是 $0$ 。

#### 最小步数模型

- 从一个图变成另一个图的最少变换步数。
- 状态数量可能是指数级别的，需要用双向广搜来优化。

#### 双端队列广搜

- 使用条件：权重只有0和1两种。

- 权重是 $0$ 的插在队头，权重是 $1$ 的插在队尾。

#### 双向广搜

- 一般应用在最小步数模型中。
- 扩展时，一般选择当前队列中数量较少的来扩展，使两方比较平衡。

#### A* (启发式搜索)

- 搜索的规模很大的时候，可使用A*优化。
- 只能保证终点出队时，终点的距离是最小的。不能保证中间点是最小的。
- 使用条件：
  1. 估计距离 $<=$ 真实距离。
  2. 一定有解，不然效率不如普通BFS。
- 使用步骤：
  1. 将队列换成优先队列。除了存真实距离，还要存真实距离与估计距离之和。
  2. 取出优先队列的队头。
  3. 扩展可以扩展的所有邻边。
  4. 当终点第一次出队时，break。

- Dijkstra算法可以看作终点到当前点的估计值是0的A*算法。

### 题目

#### 池塘计数 (Flood Fill)

- 代码

```c++
#include <iostream>
#include <cstring>
#include <algorithm>

#define fi first
#define se second

using namespace std;

typedef pair<int, int> PII;

const int N = 1010, M = N * N;

int n, m;
char g[N][N];
PII q[M];
bool st[N][N];

void bfs(int sx, int sy)
{
    int hh = 0, tt = 0;
    q[0] = {sx, sy};
    st[hh][tt] = true;
    
    while (hh <= tt)
    {
        PII u = q[hh ++ ];
        
        for (int i = u.fi - 1; i <= u.fi + 1; i ++ )
            for (int j = u.se - 1; j <= u.se + 1; j ++ )
                {
                    if (i == u.fi && j == u.se) continue;
                    if (i < 0 || i >= n || j < 0 || j >= m) continue;
                    if (g[i][j] == '.' || st[i][j]) continue;
                    
                    q[ ++ tt] = {i, j};
                    st[i][j] = true;
                }
    }
}

int main()
{
    scanf("%d%d", &n, &m);
    for (int i = 0; i < n; i ++ ) scanf("%s", g[i]);
    
    int cnt = 0;
    for (int i = 0; i < n; i ++ )
        for (int j = 0; j < m; j ++ )
            if (g[i][j] == 'W' && !st[i][j])
            {
                cnt ++ ;
                bfs(i, j);
            }
            
    printf("%d\n", cnt);
    
    return 0;
}
```



#### 城堡问题 (二进制 + Flood Fill)

```c++
#include <iostream>
#include <cstring>
#include <algorithm>

#define fi first
#define se second

using namespace std;

typedef pair<int, int> PII;

const int N = 55, M = N * N;

PII q[M];
int n, m;
int g[N][N];
bool st[N][N];

int bfs(int sx, int sy)
{
    int dx[4] = {0, -1, 0, 1}, dy[4] = {-1, 0, 1, 0};
    
    int area = 0;
    int hh = 0, tt = 0;
    
    q[hh] = {sx, sy};
    st[sx][sy] = true;
    
    while (hh <= tt)
    {
        PII u = q[hh ++ ];
        area ++ ;
        
        for (int i = 0; i < 4; i ++ )
        {
            int a = u.fi + dx[i], b = u.se + dy[i];
            if (a < 0 || a >= n || b < 0 || b >= m) continue;
            if (st[a][b]) continue;
            if (g[u.fi][u.se] >> i & 1) continue;
            
            q[ ++ tt] = {a, b};
            st[a][b] = true;
        }
    }
    
    return area;
}

int main()
{
    cin >> n >> m;
    for (int i = 0; i < n; i ++ )
        for (int j = 0; j < m; j ++ )
            cin >> g[i][j];
            
    int cnt = 0, area = 0;
            
    for (int i = 0; i < n; i ++ )
        for (int j = 0; j < m; j ++ )
            if (!st[i][j])
            {
                cnt ++ ;
                area = max(area, bfs(i, j));
            }
            
    cout << cnt << endl;
    cout << area << endl;
    
    return 0;
}
```



#### 山峰和山谷 (Flood Fill)

```c++
#include <iostream>
#include <cstring>
#include <algorithm>

#define fi first
#define se second

using namespace std;

typedef pair<int, int> PII;

const int N = 1010, M = N * N;

int n;
int g[N][N];
bool st[N][N];
PII q[M];

void bfs(int sx, int sy, bool& has_lower, bool& has_higher)
{
    int hh = 0, tt = 0;
    q[0] = {sx, sy};
    st[sx][sy] = true;
    
    while (hh <= tt)
    {
        PII u = q[hh ++ ];
        
        for (int i = u.fi - 1; i <= u.fi + 1; i ++ )
            for (int j = u.se - 1; j <= u.se + 1; j ++ )
                {
                    if (i < 0 || i >= n || j < 0 || j >= n) continue;
                    
                    if (g[i][j] != g[u.fi][u.se])
                    {
                        if (g[i][j] > g[u.fi][u.se]) has_higher = true;
                        else has_lower = true;
                    }
                    else if (!st[i][j])
                    {
                        q [ ++ tt] = {i, j};
                        st[i][j] = true;
                    }
                }
    }
}

int main()
{
    scanf("%d", &n);
    for (int i = 0; i < n; i ++ )
        for (int j = 0; j < n; j ++ )
            scanf("%d", &g[i][j]);
            
    int peak = 0, valley = 0;
    for (int i = 0; i < n; i ++ )
        for (int j = 0; j < n; j ++ )
            if (!st[i][j])
            {
                bool has_lower = false, has_higher = false;
                bfs(i, j, has_lower, has_higher);
                if (!has_lower) valley ++ ;
                if (!has_higher) peak ++ ;
            }
            
    printf("%d %d\n", peak, valley);
    
    return 0;
}
```



#### 迷宫问题 (最短路模型)

```c++
#include <cstring>
#include <iostream>
#include <algorithm>

#define x first
#define y second

using namespace std;

typedef pair<int, int> PII;

const int N = 1010, M = N * N;

int n;
int g[N][N];
PII q[M];
PII pre[N][N];
int dx[4] = {1, 0, -1, 0};
int dy[4] = {0, -1, 0, 1};

void bfs(int sx, int sy)
{
    int hh = 0, tt = 0;
    q[0] = {sx, sy};
    
    memset(pre, -1, sizeof pre);
    
    while (hh <= tt)
    {
        PII u = q[hh ++ ];
        
        for (int i = 0; i < 4; i ++ )
        {
            int a = u.x + dx[i], b = u.y + dy[i];
            
            if (a < 0 || a >= n || b < 0 || b >= n) continue;
            if (g[a][b]) continue;
            if (pre[a][b].x != -1) continue;
            
            q[ ++ tt] = {a, b};
            pre[a][b] = u;
        }
    }
}

int main()
{
    scanf("%d", &n);
    for (int i = 0; i < n; i ++ )
        for (int j = 0; j < n; j ++ )
            scanf("%d", &g[i][j]);
    
    bfs(n - 1, n - 1);
    
    PII end(0, 0);
    
    while (true)
    {
        printf("%d %d\n", end.x, end.y);
        if (end.x == n - 1 && end.y == n - 1) break;
        end = pre[end.x][end.y];
    }
    
    return 0;
}
```



#### 武士风度的牛 (最短路模型)

```c++
#include <iostream>
#include <cstring>
#include <algorithm>

#define x first
#define y second

using namespace std;

typedef pair<int, int> PII;

const int N = 150, M = N * N;

int n, m;
PII q[M];
char g[N][N];
int dist[N][N];
int dx[8] = {-2, -1, 1, 2, 2, 1, -1, -2};
int dy[8] = {1, 2, 2, 1, -1, -2, -2, -1};

int bfs()
{
    int sx, sy;
    for (int i = 0; i < n; i ++ )
        for (int j = 0; j < m; j ++ )
            if (g[i][j] == 'K')
            {
                sx = i, sy = j;
                break;
            }
            
    int hh = 0, tt = 0;
    q[0] = {sx, sy};
    
    memset(dist, -1, sizeof dist);
    dist[sx][sy] = 0;
    
    while (hh <= tt)
    {
        PII u = q[hh ++ ];
        
        for (int i = 0; i < 8; i ++ )
        {
            int a = u.x + dx[i], b = u.y + dy[i];
            
            if (a < 0 || a >= n || b < 0 || b >= m) continue;
            if (g[a][b] == '*') continue;
            if (dist[a][b] != -1) continue;
            if (g[a][b] == 'H')  return dist[u.x][u.y] + 1;
            
            q[ ++ tt] = {a, b};
            dist[a][b] = dist[u.x][u.y] + 1;
        }
    }
    
    return -1;
}

int main()
{
    cin >> m >> n;
    for (int i = 0; i < n; i ++ ) cin >> g[i];
    
    cout << bfs() << endl;
    
    return 0;
}
```



####  抓住那头牛  (最短路模型)

```c++
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

const int N = 1e5 + 10;

int n, k;
int q[N];
int dist[N];

int bfs()
{
    memset(dist, -1, sizeof dist);
    dist[n] = 0;
    q[0] = n;
    
    int hh = 0, tt = 0;
    while (hh <= tt)
    {
        int u = q[hh ++ ];
        
        if (u == k) return dist[k];
        
        if (u + 1 < N && dist[u + 1] == -1) 
        {
            q[ ++ tt] = u + 1;
            dist[u + 1] = dist[u] + 1;
        }
        if (u - 1 >= 0 && dist[u - 1] == -1)
        {
            q[ ++ tt] = u - 1;
            dist[u - 1] = dist[u] + 1;
        }
        if (u * 2 < N && dist[u * 2] == -1)
        {
            q[ ++ tt] = u * 2;
            dist[u * 2] = dist[u] + 1;
        }
    }
    
    return -1;
}

int main()
{
    cin >> n >> k;
    
    cout << bfs() << endl;
    
    return 0;
}
```



#### 矩阵距离 (多源BFS)

```c++
#include <iostream>
#include <cstring>
#include <algorithm>

#define x first
#define y second

using namespace std;

typedef pair<int, int> PII;

const int N = 1010, M = N * N;

int n, m;
char g[N][N];
PII q[M];
int dist[N][N];
int dx[] = {1, 0, -1, 0};
int dy[] = {0, 1, 0, -1};

void bfs()
{
    int hh = 0, tt = -1;
    memset(dist, -1, sizeof dist);
    
    for (int i = 0; i < n; i ++ )
        for (int j = 0; j < m; j ++ )
            if (g[i][j] == '1')
            {
                dist[i][j] = 0;
                q[ ++ tt] = {i, j};
            }
    
    while (hh <= tt)
    {
        PII u = q[hh ++ ];
        
        for (int i = 0; i < 4; i ++ )
        {
            int a = u.x + dx[i], b = u.y + dy[i];
            
            if (a < 0 || a >= n || b < 0 || b >= m) continue;
            if (dist[a][b] != -1 ) continue;
            
            q[ ++ tt] = {a, b};
            dist[a][b] = dist[u.x][u.y] + 1;
        }

    }
}

int main()
{
    scanf("%d%d", &n, &m);
    for (int i = 0; i < n; i ++ ) scanf("%s", g[i]);
    
    bfs();
    
    for (int i = 0; i < n; i ++ )
        for (int j = 0; j < m; j ++ )
            printf("%d%c", dist[i][j], j == m - 1 ? '\n' : ' ');
    
    return 0;
}
```



#### 魔板 (最小步数模型)

```c++
#include <iostream>
#include <cstring>
#include <algorithm>
#include <unordered_map>
#include <queue>

#define x first
#define y second

using namespace std;

char g[2][4];
unordered_map<string, int> dist;
unordered_map<string, pair<char, string> > pre; 
queue<string> q;

void set(string state)
{
    for (int i = 0; i < 4; i ++ ) g[0][i] = state[i];
    for (int i = 3, j = 4; i >= 0; i --, j ++ ) g[1][i] = state[j];
}

string get()
{
    string res;
    for (int i = 0; i < 4; i ++ ) res += g[0][i];
    for (int i = 3; i >= 0; i -- ) res += g[1][i];
    return res;
}

string move2(string state)
{
    set(state);
    
    char v = g[0][1];
    g[0][1] = g[1][1];
    g[1][1] = g[1][2];
    g[1][2] = g[0][2];
    g[0][2] = v;
    
    return get();
}

string move1(string state)
{
    set(state);
    char v0 = g[0][3], v1 = g[1][3];
    
    for (int i = 3; i > 0; i -- )
        for (int j = 0; j < 2; j ++ )
            g[j][i] = g[j][i - 1];
    
    g[0][0] = v0, g[1][0] = v1;
    
    return get();
}

string move0(string state)
{
    set(state);
    for (int i = 0; i < 4; i ++ ) swap(g[1][i], g[0][i]);
    return get();
}

void bfs(string start, string end)
{
    if (start == end) return;
    
    q.push(start);
    
    while (q.size())
    {
        string u = q.front();
        q.pop();
        
        string m[3];
        
        m[0] = move0(u);
        m[1] = move1(u);
        m[2] = move2(u);
        
        for (int i = 0; i < 3; i ++ )
        {
            if (dist.count(m[i])) continue;
            
            dist[m[i]] = dist[u] + 1;
            pre[m[i]] = {'A' + i, u};
            
            if (m[i] == end) return;
            
            q.push(m[i]);
        }
    }
}

int main()
{
    int x;
    string start, end;
    for (int i = 0; i < 8; i ++ )
    {
        cin >> x;
        end += (x + '0');
    }
    for (int i = 0; i < 8; i ++ ) start += i + '1';
    
    bfs(start, end);
    
    cout << dist[end] << endl;
    
    string res;
    while (end != start)
    {
        res += pre[end].x;
        end = pre[end].y;
    }
    
    reverse(res.begin(), res.end());
    
    if (res.size()) cout << res << endl;
    
    return 0;
}
```

 

#### 电路维修 (双端队列BFS)

```c++
#include <iostream>
#include <cstring>
#include <algorithm>
#include <deque>

using namespace std;

typedef pair<int, int> PII;

const int N = 510, M = N * N;

int n, m;
char g[N][N];
int dist[N][N];
bool st[N][N];

int bfs()
{
    memset(st, 0, sizeof st);
    memset(dist, 0x3f, sizeof dist);
    dist[0][0] = 0;
    
    deque<PII> q;
    q.push_back({0, 0});
    
    int dx[4] = {-1, -1, 1, 1}, dy[4] = {-1, 1, 1, -1};
    int ix[4] = {-1, -1, 0, 0}, iy[4] = {-1, 0, 0, -1};
    char cs[] = "\\/\\/";
    
    while (q.size())
    {
        PII u = q.front();
        q.pop_front();
        
        int x = u.first, y = u.second;
        
        if (st[x][y]) continue;
        st[x][y] = true;
        
        for (int i = 0; i < 4; i ++ )
        {
            int a = x + dx[i], b = y + dy[i];
            
            if (a < 0 || a > n || b < 0 || b > m) continue;
            
            int ca = x + ix[i], cb = y + iy[i];
            int d = dist[x][y] + (cs[i] != g[ca][cb]);
            
            if (d < dist[a][b])
            {
                dist[a][b] = d;
                
                if (g[ca][cb] != cs[i] ) q.push_back({a, b});
                else q.push_front({a, b});
            }
        }
    }
    
    return dist[n][m];
}

int main()
{
    int T;
    scanf("%d", &T);
    
    while (T -- )
    {
        scanf("%d%d", &n, &m);
        for (int i = 0; i < n; i ++ ) scanf("%s", g[i]);
        
        if ((n + m) & 1) puts("NO SOLUTION");
        else cout << bfs() << endl;
    }
    
    return 0;
}
```



#### 字串变换 (双向广搜)

```c++
#include <iostream>
#include <cstring>
#include <algorithm>
#include <queue>
#include <unordered_map>

using namespace std;

int n;
string a[6], b[6];

int extend(queue<string>& q, unordered_map<string, int>& da, 
unordered_map<string, int>& db, string a[], string b[])
{
    int d = da[q.front()];
    
    while (q.size() && d == da[q.front()])
    {
        string u = q.front();
        q.pop();
        
        for (int i = 0; i < u.size(); i ++ )
            for (int j = 0; j < n; j ++ )
            {
                if (u.substr(i, a[j].size()) == a[j])
                {
                    string t = u.substr(0, i) + b[j] + u.substr(i + a[j].size());
                    
                    if (db.count(t)) return 1 + da[u] + db[t];
                    if (da.count(t)) continue;
                    da[t] = da[u] + 1;
                    q.push(t);
                }
            }
    }
    

    return 11;
}

int bfs(string A, string B)
{
    if (A == B) return 0;
    
    queue<string> qa, qb;
    unordered_map<string, int> da, db;
    
    qa.push(A), da[A] = 0;
    qb.push(B), db[B] = 0;
    
    while (qa.size() && qb.size())
    {
        int t;
        if (qa.size() <= qb.size()) t = extend(qa, da, db, a, b);
        else t = extend(qb, db, da, b, a);
        
        if (t <= 10) return t;
    }
    
    return 11;
}

int main()
{
    string A, B;
    cin >> A >> B;
    while (cin >> a[n] >> b[n]) n ++ ;

    int t = bfs(A, B);
    
    if (t <= 10) cout << t << endl;
    else cout << "NO ANSWER!" << endl;
    
    return 0;
}
```



#### 八数码 (A*)

```c++
#include <iostream>
#include <cstring>
#include <algorithm>
#include <unordered_map>
#include <queue>

#define x first
#define y second

using namespace std;

typedef pair<int ,string> PIS;

int f(string state)
{
    int res = 0;
    for (int i = 0; i < state.size(); i ++ )
        if (state[i] != 'X')
        {
            int t = state[i] - '1';
            res += abs(i / 3 - t / 3) + abs(i % 3 - t % 3);
        }
    return res;
}

string bfs(string start)
{
    string end = "12345678x";
    priority_queue<PIS, vector<PIS>, greater<PIS> > heap;
    unordered_map<string, int> dist;
    unordered_map<string, pair<char, string> > pre;
    
    int dx[] = {1, 0, 0, -1}, dy[] = {0, -1, 1, 0};
    char op[] = "dlru";
    
    dist[start] = 0;
    
    heap.push({f(start), start});
    while (heap.size())
    {
        auto t = heap.top();
        heap.pop();
        
        string state = t.y;
        
        if (state == end) break;
        
        int x, y;
        for (int i = 0; i < state.size(); i ++ )
            if (state[i] == 'x')
            {
                x = i / 3;
                y = i % 3;
                break;
            }
            
        int step = dist[state];
        string src = state;
        
        for (int i = 0; i < 4; i ++ )
        {
            int a = x + dx[i], b = y + dy[i];
            if (a < 0 || a >= 3 || b < 0 || b >= 3) continue;
              
            swap(state[a * 3 + b], state[x * 3 + y]);
            if (!dist.count(state) || dist[state] > step + 1)
            {
                dist[state] = dist[src] + 1;
                pre[state] = {op[i], src};
                heap.push({f(state) + dist[state], state});
            }
            swap(state[a * 3 + b], state[x * 3 + y]);
        }
    }
    
    string res;
    while (end != start)
    {
        res += pre[end].x;
        end = pre[end].y;
    }
    
    reverse(res.begin(), res.end());
    
    return res;
}

int main()
{
    string start, seq;
    char c;
    while (cin >> c)
    {
        start += c;
        if (c != 'x') seq += c;
    }
    
    int cnt = 0;
    for (int i = 0; i < 8; i ++ )
        for (int j = i; j < 8; j ++ )
            if (seq[j] > seq[i]) cnt ++ ;
            
    if (cnt & 1) cout << "unsolvable" << endl;
    else cout << bfs(start) << endl;
    
    return 0;
}
```



#### 第K短路 (A*)

- 估价函数：每个点到终点的距离。

```c++
#include <iostream>
#include <cstring>
#include <algorithm>
#include <queue>

#define x first
#define y second

using namespace std;

typedef pair<int, int> PII;
typedef pair<int, PII> PIII;

const int N = 1010, M = 2e5 + 10, INF = 0x3f3f3f3f;

int h[N], rh[N], e[M], ne[M], w[M], idx;
int n, m, S, T, K;
int dist[N]; // astar估计函数
bool st[N]; // dijkstra判重

// 添边
void add(int h[], int a, int b, int c)
{
    w[idx] = c, e[idx] = b, ne[idx] = h[a], h[a] = idx ++ ;
}

void dijkstra()
{
    priority_queue<PII, vector<PII>, greater<PII> > heap;
    heap.push({0, T});
    
    memset(dist, 0x3f, sizeof dist);
    dist[T] = 0;
    
    while (heap.size())
    {
        auto t = heap.top();
        heap.pop();
        
        int ver = t.y;
        
        if (st[ver]) continue;
        st[ver] = true;
        
        for (int i = rh[ver]; ~i; i = ne[i])
        {
            int j = e[i];
            
            if (dist[j] > dist[ver] + w[i])
            {
                dist[j] = dist[ver] + w[i];
                heap.push({dist[j], j});
            }
        }
    }
}

int astar()
{
    priority_queue<PIII, vector<PIII>, greater<PIII> > heap;
    heap.push({dist[S], {0, S}});
    
    int cnt = 0;
    
    while (heap.size())
    {
        auto t = heap.top();
        heap.pop();
        
        int ver = t.y.y, dis = t.y.x;
        
        if (ver == T) cnt ++ ;
        if (cnt == K) return dis;
        
        for (int i = h[ver]; ~i; i = ne[i])
        {
            int j = e[i];
            if (dist[j] != INF)
                heap.push({dist[j] + dis + w[i], {dis + w[i], j}});
        }
    }
    
    return -1;
}

int main()
{
    scanf("%d%d", &n, &m);
    memset(h, -1, sizeof h);
    memset(rh, -1, sizeof rh);
    
    for (int i = 0; i < m; i ++ )
    {
        int a, b, c;
        scanf("%d%d%d", &a, &b, &c);
        add(h, a, b, c);
        add(rh, b, a, c);
    }
    
    scanf("%d%d%d", &S, &T, &K);
    
    dijkstra();
    
    if (S == T) K ++ ;
    cout << astar() << endl;
    
    return 0;
}
```

