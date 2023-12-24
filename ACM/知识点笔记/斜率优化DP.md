## 	斜率优化DP

### 背景

- 一般DP问题：
  - 分析 ---> 保证正确性。
  - 优化 ---> 等价变形，注重效率。
- 斜率优化的DP问题一般转移方程比较复杂。



---



### AcWing 任务安排1

- 费用提前处理思想。

- 闫氏DP分析

  - 状态表示：

    - 集合：`f[i]` 表示将前 `i` 个物品处理完的所有方案的集合。

    - 属性：`Min`

  - 状态计算：根据最后一批的上一个任务不同来划分 

    - 把变化的和不变化的分开看。

    - 预处理出 `T` 和 `C` 的前缀和。

    - `f[i] = sumT[i] * (sumC[i] - sumC[j]) + S * (sumC[n] - sumC[j]) + f[j]`

       ($0<=j<=i-1$)



```c++
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

const int N = 5010;

typedef long long LL;

LL n, s;
LL sumt[N], sumc[N];
LL f[N];

int main()
{
    cin >> n >> s;
    for (int i = 1; i <= n; i ++ )
    {
        LL t, c;
        cin >> t >> c;
        sumt[i] = sumt[i - 1] + t;
        sumc[i] = sumc[i - 1] + c;
    }
    
    memset(f, 0x3f, sizeof f);
    f[0] = 0;
    
    for (int i = 1; i <= n; i ++ )
        for (int j = 0; j < i; j ++ )
            f[i] = min(f[i], f[j] + sumt[i] * (sumc[i] - sumc[j]) + s * (sumc[n] - sumc[j]));
            
    cout << f[n] << endl;
    
    return 0;
}
```



----



### AcWing 任务安排2

- 使用斜率优化。
- 针对某一个 `f[i]` 而言，变量只有 `j` ，划分出来 `x` 和 `y` 即自变量和因变量，因为斜率固定，找截距与 `f[i]` 的关系，发现截距越小， `f[i]` 越小。
- 在本题中，凸包上下边界第一个斜率大于 `k` 的点，是需要的点。
- 斜率单调递增，新加的点的横坐标也是单调递增。
- 在查询的时候，将队头小于当前斜率的点全部删掉。
- 在插入的时候，将队尾所有不在凸包上的点全部删掉。

```c++
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

typedef long long LL;

const int N = 3e5 + 10;

int n, s;
LL t[N], c[N];
LL f[N];
int q[N];

int main()
{
    scanf("%d%d", &n, &s);
    for (int i = 1; i <= n; i ++ )
    {
        scanf("%lld%lld", &t[i], &c[i]);
        t[i] += t[i - 1];
        c[i] += c[i - 1];
    }
    
    int hh = 0, tt = 0;
    q[0] = 0;
    for (int i = 1; i <= n; i ++ )
    {
        while (hh < tt && (f[q[hh + 1]] - f[q[hh]]) <= (t[i] + s) * (c[q[hh + 1]] - c[q[hh]])) hh ++ ;
        f[i] = f[q[hh]] + t[i] * (c[i] - c[q[hh]]) + s * (c[n] - c[q[hh]]);
        while (hh < tt && (f[q[tt]] - f[q[tt - 1]]) * (c[i] - c[q[tt]]) >= (f[i] - f[q[tt]]) * (c[q[tt]] - c[q[tt - 1]])) tt -- ;
        q[++ tt] = i;
    }
    
    printf("%lld\n", f[n]);
    
    return 0;
}
```



----



### AcWing 任务安排3

- 斜率不再具有单调性，但新加的点的横坐标一定单调递增。
  - 查询的时候，只能二分。
  - 插入的时候，将队尾不在凸包上的点全部删除。

```c++
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

typedef long long LL;

const int N = 3e5 + 10;

int n, s;
LL t[N], c[N], f[N];
int q[N];

int main()
{
    scanf("%d%d", &n, &s);
    for (int i = 1; i <= n; i ++ )
    {
        scanf("%lld%lld", &t[i], &c[i]);
        t[i] += t[i - 1];
        c[i] += c[i - 1];
    }
    
    int hh = 0, tt = 0;
    for (int i = 1; i <= n; i ++ )
    {
        int l = hh, r = tt;
        while (l < r)
        {
            int mid = l + r >> 1;
            if ((f[q[mid + 1]] - f[q[mid]]) >= (t[i] + s) * (c[q[mid + 1]] - c[q[mid]])) r = mid;
            else l = mid + 1;
        }
        
        
        int j = q[r];
        f[i] = f[j] + t[i] * (c[i]  - c[j]) + s * (c[n] - c[j]);
        while (hh < tt && (double)(f[q[tt]] - f[q[tt - 1]]) * (c[i] - c[q[tt]]) >= (double)(f[i] - f[q[tt]]) * (c[q[tt]] - c[q[tt - 1]])) tt -- ;
        q[++ tt] = i;
    }
    
    printf("%lld\n", f[n]);
    
    return 0;
}
```



------



### AcWing 运输小猫

-  `d[i]` 存前缀和，表示 `1 ~ i` 的距离。
- `s[i]` 饲养员的出发时间。
- `A[i] = t[i] - d[i]` ，接上小猫的饲最早出发时间。
- 接到小猫的充要条件：$s_i + d_i >=  t_i$ ， 即 $s_i >= t_i - d_i$。
- 抽象出来的模型：给定 `m` 个从小到大排好序的数，最多把这个序列分成 `p` 组，每组的每个数的花费是当前数减去最后数的差的和，求最少花费。
- `f[j][i]` ：用 `j` 个饲养员，取前 `i` 只小猫的最小花费。
- 转移方程：

`f[j][i] = min(f[j - 1][k] + a[i] * (i - k) - (S[i] - S[k]))` 

- 化简得到：

`f[j-1][k] + S[k] = a[i] * k - f[j][i] - a[i] * i + S[i]`

- 代码

```c++
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

typedef long long LL;

const int N = 1e5 + 10, M = 1e5 + 10, P = 100 + 10;

int n, m, p;
LL t[M], d[N], a[M], s[M];
int q[M];
LL f[P][M];

LL get_y(int k, int j)
{
    return f[j - 1][k] + s[k];
}

int main()
{
    scanf("%d%d%d", &n, &m, &p);
    
    for (int i = 2; i <= n; i ++ )
    {
        scanf("%lld", &d[i]);
        d[i] += d[i - 1];
    }
    
    for (int i = 1; i <= m; i ++ )
    {
        int h;
        scanf("%d%lld", &h, &t[i]);
        a[i] = t[i] - d[h];
    }
    
    sort(a + 1, a + 1 + m);
    
    for (int i = 1; i <= m; i ++ ) s[i] = s[i - 1] + a[i];
    
    memset(f, 0x3f, sizeof f);
    for (int i = 0; i <= p; i ++ ) f[i][0] = 0;
    
    for (int j = 1; j <= p; j ++ )
    {
        int hh = 0, tt = 0;
        q[0] = 0;
        
        for (int i = 1; i <= m; i ++ )
        {
            while (hh < tt && (get_y(q[hh + 1], j) - get_y(q[hh], j)) <=
            a[i] * (q[hh + 1] - q[hh])) hh ++ ;
            
            int k = q[hh];
            f[j][i] = f[j - 1][k] - a[i] * k + s[k] + a[i] * i - s[i];
            
            while (hh < tt && (get_y(q[tt], j) - get_y(q[tt - 1], j)) * (i - q[tt]) >=
            (get_y(i, j) - get_y(q[tt], j)) * (q[tt] - q[tt - 1])) tt -- ;
            
            q[ ++ tt] = i;
        }
    }
    
    printf("%lld\n", f[p][m]);
    
    return 0;
}
```





