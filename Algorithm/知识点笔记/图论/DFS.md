### 介绍

#### BFS 与 DFS

- BFS一层一层向外扩展
- DFS一条路走到头。

#### 恢复现场问题

- 内部搜索，不需要恢复现场。
- 外部搜索，需要恢复现场。

#### 关于证明

- 不能凭直觉来判断，要严谨地从数学角度推理。

### 算法

#### DFS之连通性模型

- 缺点：只能判断是否联通，并不能保证是最短。
- 优点：代码短。

#### DFS之搜索顺序

- 使用DFS暴搜时，需要考虑一个顺序把所有方案都考虑到。
- 顺序非常重要，必须要找到一个顺序，可以枚举到所有方案。

#### 剪枝

- 优化搜索顺序
  - 优先搜索==分支少==的节点。
- 排除等效冗余 
- 可行性剪枝
  - 如果当前路径不可行，直接返回。
- 最优性剪枝
  - 如果当前路径结果不是最优的，直接返回。
- 记忆化搜索 (DP)

#### 迭代加深

- 处理答案在比较浅的层中，而可能会搜到很深的层中的情况。

- max_depth
  - 最大搜索区域，超过这个区域的剪掉。

#### 双向DFS

#### IDA*

- 一般配合迭代加深使用。
- 如果当前路径会超过 `max_depth` 直接剪枝。

### 例题

#### 迷宫 (连通性模型)

```c++
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

const int N = 110;

int n;
char g[N][N];
bool st[N][N];
int sx, sy, ex, ey;

int dx[] = {0, 1, 0, -1}, dy[] = {1, 0, -1, 0};

bool dfs(int x, int y)
{
    if (g[x][y] == '#') return false;
    
    if (x == ex && y == ey) return true;
    
    st[x][y] = true;
    
    for (int i = 0; i < 4; i ++ )
    {
        int a = x + dx[i], b = y + dy[i];
        
        if (a < 0 || a >= n || b < 0 || b >= n) continue;
        if (st[a][b]) continue;
        
        if (dfs(a, b)) return true;
    }
    return false;
}

int main()
{
    int T;
    cin >> T;
    
    while (T -- )
    {
        cin >> n;
        for (int i = 0; i < n; i ++ ) cin >> g[i];
        cin >> sx >> sy >> ex >> ey;
        memset(st, 0, sizeof st);
        
        if (dfs(sx, sy)) cout << "YES" << endl;
        else cout << "NO" << endl;
    }
    
    return 0;
}
```

#### 红与黑 (连通性模型)

```c++
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

const int N = 25;

int n, m;
char g[N][N];
bool st[N][N];
int dx[] = {0, 1, 0, -1}, dy[] = {1, 0, -1, 0};
int dfs(int x, int y)
{
    int cnt = 1;
    st[x][y] = true;
    
    for (int i = 0; i < 4; i ++ )
    {
        int a = x + dx[i];
        int b = y + dy[i];
        
        if (a < 0 || a >= n || b < 0 || b >= m) continue;
        if (g[a][b] != '.') continue;
        if (st[a][b]) continue;
        
        cnt += dfs(a, b);
    }
    
    return cnt;
}

int main()
{
    while (cin >> m >> n, n || m)
    {
        int x, y;
        for (int i = 0; i < n; i ++ )
            for (int j = 0; j < m; j ++ )
            {
                cin >> g[i][j];
                if (g[i][j] == '@')
                {
                    x = i, y = j;
                }
            }
            
        memset(st, 0, sizeof st);
        cout << dfs(x, y) << endl;
    }
    return 0;
}
```

#### 马走日 (搜索顺序)

```c++
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

const int N = 12;

int ans;
int n, m, x, y;
bool st[N][N];

int dx[] = {-2, -1, 1, 2, 2, 1, -1, -2};
int dy[] = {1, 2, 2, 1, -1, -2, -2, -1};

void dfs(int x, int y, int cnt)
{
    if (cnt == n * m)
    {
        ans ++ ;
        return;
    }
    
    st[x][y] = true;
    
    for (int i = 0; i < 8; i ++ )
    {
        int a = x + dx[i], b = y + dy[i];
        
        if (a < 0 || a >= n || b < 0 || b >= m) continue;
        if (st[a][b]) continue;
        
        dfs(a, b, cnt + 1);
    }
    
    st[x][y] = false;
}

int main()
{
    int T;
    cin >> T;
    
    while (T -- )
    {
        cin >> n >> m >> x >> y;
        
        ans = 0;
        dfs(x, y, 1);
        
        cout << ans << endl;
    }
    
    return 0;
}
```

#### 单词接龙 (搜索顺序)

```c++
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

const int N = 20 + 10;

int n;
int ans;
int g[N][N];
int used[N];
char start;
string word[N];

void dfs(string dragon, int id)
{
    ans = max(ans, (int)dragon.size());
    
    used[id] ++ ;
    
    for (int i = 0; i < n; i ++ )
        if (used[i] < 2 && g[id][i])
            dfs(dragon + word[i].substr(g[id][i]), i);
    
    used[id] -- ;
        
}

int main()
{
    cin >> n;
    for (int i = 0; i < n; i ++ ) cin >> word[i];
    cin >> start;
    
    // 预处理g
    for (int i = 0; i < n; i ++ )
        for (int j = 0; j < n; j ++ )
        {
            string a = word[i], b = word[j];
            for (int k = 1; k < min(a.size(), b.size()); k ++ )
                if (a.substr(a.size() - k, k) == b.substr(0, k))
                {
                    g[i][j] = k;
                    break;
                }
        }
            
    //搜索
    for (int i = 0; i < n; i ++ )
        if (word[i][0] == start)
            dfs(word[i], i);
    
    cout << ans << endl;
    
    return 0;
}
```

#### 小猫爬山 (剪枝)

```c++
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

const int N = 20;

int n, m;
int s[N], w[N];
int ans = N;

void dfs(int u, int k)
{
    if (k >= ans) return;
    if (u == n)
    {
        ans = k;
        return;
    }
    
    for (int i = 0; i < k; i ++ )
        if (s[i] + w[u] <= m)
        {
            s[i] += w[u];
            dfs(u + 1, k);
            s[i] -= w[u];
        }
    
    s[k] += w[u];
    dfs(u + 1, k + 1);
    s[k] -= w[u];
}

int main()
{
    cin >> n >> m;
    
    for (int i = 0; i < n; i ++ ) cin >> w[i];
    
    sort(w, w + n);
    reverse(w, w + n);
    
    dfs(0, 0);
    
    cout << ans << endl;
    
    return 0;
}
```

#### 数独 (剪枝)

```c++
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

const int N = 9, M = 1 << N;

int ones[M], map[M];
char str[100];
int row[N], col[N], cell[3][3];

void init()
{
    for (int i = 0; i < N; i ++ ) row[i] = col[i] = (1 << N) - 1;
    
    for (int i = 0; i < 3; i ++ )
        for (int j = 0; j < 3; j ++ )
            cell[i][j] = (1 << N) - 1;
}

int get(int x, int y)
{
    return row[x] & col[y] & cell[x / 3][y / 3];
}

int lowbit(int x)
{
    return x & -x;
}

void draw(int x, int y, int t, bool is_set)
{
    if (is_set) str[x * N + y] = '1' + t;
    else str[x * N + y] = '.';
    
    int v = 1 << t;
    if (!is_set) v = -v;
    
    row[x] -= v;
    col[y] -= v;
    cell[x / 3][y / 3] -= v;
}

bool dfs(int cnt)
{
    if (!cnt) return true;
    
    int minv = 10;
    int x, y;
    for (int i = 0; i < N; i ++ )
        for (int j = 0; j < N; j ++ )
            if (str[i * N + j] == '.')
            {
                int state = get(i, j);
                if (ones[state] < minv)
                {
                    minv = ones[state];
                    x = i, y = j;
                }
            }
            
    int state = get(x, y);
    for (int i = state; i; i -= lowbit(i))
    {
        int t = map[lowbit(i)];
        draw(x, y, t, true);
        if (dfs(cnt - 1)) return true;
        draw(x, y, t, false);
    }
    
    return false;
}

int main()
{
    for (int i = 0; i < N; i ++ ) map[1 << i] = i;
    for (int i = 0; i < (1 << N); i ++ )
        for (int j = 0; j < N; j ++ )
            ones[i] += (i >> j) & 1;
            
    while (cin >> str, str[0] != 'e')
    {
        init();
        
        int cnt = 0;
        for (int i = 0, k = 0; i < N; i ++ )
            for (int j = 0; j < N; j ++, k ++ )
                if (str[k] != '.') 
                    draw(i, j, str[k] - '1' , true);
                else cnt ++ ;
                
        dfs(cnt);
        
        puts(str);
    }
    return 0;
}
```

#### 木棒 (剪枝)

- 剪枝优化：
  - len 整除 sum
  - 从大到小枚举
  - 按照组合数枚举
  - 当前木棍加到当前木棒中失败了，直接略过后面相等长度的木棍
  - 如果木棒的第一根木棍失败，则一定失败，直接回溯。
  - 如果木棒的最后一根木棍失败，则一定失败

```c++
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

const int N = 70;

int n;
int w[N], length, sum;
bool st[N];

// u: 当前木棍数量
// s: 当前木棍长度
// start: 从哪根木棍开始枚举
bool dfs(int u, int s, int start)
{
    // u 从0开始计数，如果当前是u，那么已经有u根满足情况。
    if (u * length == sum) return true;
    
    // 当前木棍长度满足条件，返回新开一根木棍
    if (s == length) return dfs(u + 1, 0, 0);
    
    // 剪枝3
    for (int i = start; i < n; i ++ )
    {
        if (st[i] || s + w[i] > length) continue;
        
        st[i] = true;
        if (dfs(u, s + w[i], i + 1)) return true;
        st[i] = false;
        
        // 剪枝5、6
        if (!s || s + w[i] == length) return false;
        
        // 剪枝4
        int j = i;
        while (w[j] == w[i] && j < n) j ++ ;
        i = j - 1;
    }
    return false;
}

int main()
{
    while (cin >> n, n)
    {
        sum = 0;
        memset(st, 0, sizeof st);
        
        for (int i = 0; i < n; i ++ )
        {
            cin >> w[i];
            sum += w[i];
        }
        
        // 剪枝2 从大到小枚举
        sort(w, w + n);
        reverse(w, w + n);
        
        length = 1;
        while (true)
        {
            // 剪枝1 整除
            if (sum % length == 0 && dfs(0, 0, 0)) 
            {
                cout << length << endl;
                break;
            }
            length ++ ;
        }
        
    }
    
    return 0;
}
```

#### 生日蛋糕 (剪枝)

```c++
#include <iostream>
#include <cstring>
#include <algorithm>
#include <cmath>

using namespace std;

const int N = 25, INF = 1e9;

int n, m;
int minv[N], mins[N];
int R[N], H[N];
int ans = INF;

// 层数 当前已经用的体积 当前已经用的面积
void dfs(int u, int v, int s)
{
    // 可行性剪枝 最优性剪枝
    if (v + minv[u] > n || s + mins[u] >= ans) return;
    // 最优性剪枝
    if (s +  2 * (n - v) / R[u + 1] >= ans) return;
    
    // 第一层已经放好
    if (!u) 
    {
        // 如果体积正好是n，才能更新
        if (v == n) ans = s;
        return;
    }
    
    for (int r = min(R[u + 1] - 1, (int)sqrt(n - v)); r >= u; r -- )
        for (int h = min(H[u + 1] - 1, (n - v) / r / r); h >= u; h -- )
            {
                int t = 0;
                if (u == m) t = r * r;
                
                R[u] = r, H[u] = h;
                
                dfs(u - 1, v + r * r * h, s + t + 2 * r * h);
            }
}

int main()
{
    // 体积 层数
    cin >> n >> m;
    
    // 预处理前缀最小体积，面积
    for (int i = 1; i <= m; i ++ )
    {
        minv[i] = minv[i - 1] + i * i * i;
        mins[i] = mins[i - 1] + 2 * i * i;
    }
    
    // 最底层的边界
    R[m + 1] = H[m + 1] = INF;
    
    dfs(m, 0, 0);
    
    if (ans == INF) ans = 0;
    
    cout << ans << endl;
    
    return 0;
}
```

#### 加成序列 (迭代加深)

- 剪枝1：优先枚举较大的数
- 剪枝2：排除等效冗余

```cpp
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

int n;
int path[110];

bool dfs(int u, int depth)
{
    // 迭代加深剪枝
    if (u > depth) return false;
    
    // 找到答案
    if (path[u - 1] == n) return true;
    
    // 判重数组
    bool st[110] = {0};
    
    for (int i = u - 1; i >= 0; i -- )
        for (int j = i; j >= 0; j -- )
            {
                int s = path[i] + path[j];
                if (s > n || s <= path[u - 1] || st[s]) continue;
                
                st[s] = true;
                path[u] = s;
                if (dfs(u + 1, depth)) return true;
            }
    return false;
}

int main()
{
    // 第一个数是1
    path[0] = 1;
    
    while (cin >> n, n)
    {
        int depth = 1;    
        while (!dfs(1, depth)) depth ++ ;
        for (int i = 0; i < depth; i ++ )
            cout << path[i] << ' ';
        cout << endl;
    }
    
    return 0;
}
```

#### 送礼物 (双向DFS)

```cpp
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

typedef long long LL;

const int N = 50;

int n, m, k; 
int w[N], weights[1 << 24]; // 2 ^ 23 种情况
int cnt, ans;

void dfs1(int u, int s)
{
    if (u == k)
    {
        weights[cnt ++ ] = s;
        return;
    }
    
    if ((LL) s + w[u] <= m) dfs1(u + 1, s + w[u]);
    dfs1(u + 1, s);
}

void dfs2(int u, int s)
{
    if (u == n)
    {
        int l = 0, r = cnt - 1;
        while (l < r)
        {
            int mid = l + r + 1 >> 1;
            if (weights[mid] <= m - s) l = mid;
            else r = mid - 1;
        }
        
        if ((LL)s + weights[l] <= m) ans = max(ans, s + weights[l]);
        return;
    }
    
    if ((LL)s + w[u] <= m) dfs2(u + 1, s + w[u]);
    dfs2(u + 1, s);
}

int main()
{
    cin >> m >> n;
    
    for (int i = 0; i < n; i ++ ) cin >> w[i];
    
    sort(w, w + n);
    reverse(w, w + n);
    
    k = n / 2;
    
    dfs1(0, 0);
    
    sort(weights, weights + cnt);
    cnt = unique(weights, weights + cnt) - weights;
    
    dfs2(k, 0);
    
    cout << ans << endl;
    
    return 0;
}
```

