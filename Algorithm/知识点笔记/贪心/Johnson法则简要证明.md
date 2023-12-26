> 参考资料：
>
> - [Johnson法则证明](https://www.cnblogs.com/fashpoint/p/11309412.html)
> - [题解 P1248 【加工生产调度】](https://www.luogu.com.cn/blog/79017/solution-p1248)

#### 问题背景

某工厂收到了 $n$ 个产品的订单，这 $n$ 个产品分别在 $A$，$B$ 两个车间加工，并且必须先在 $A$ 车间加工后才可以到 $B$ 车间加工。

某个产品 $i$ 在 $A$、$B$ 两车间加工的时间分别为 $A_i$、$B_i$。怎样安排这 $n$ 个产品的加工顺序，才能使总的加工时间最短。

这里所说的加工时间是指：从开始加工第一个产品到最后所有的产品都已在 $A$、$B$ 两车间加工完毕的时间。

#### 简要证明

考虑我们只有两个产品的情况。

产品 $1$ ：在 $A$ 机器上加工时间为 $a_1$ ，在 $B$ 机器上加工时间为 $b_1$ 。

产品 $2$ ：在 $A$ 机器上加工时间为 $a_2$，在 $B$ 机器上加工时间为 $b_2$ 。

==假设产品 $1$ 先加工是总加工时间最小的。==

若先加工产品 $1$ ，总时间为 $a_1+max\{a_2,b_1\}+b_2$ 。

 若先加工产品 $2$ ，总时间为 $a_2+max\{a_1,b_2\}+b_1$ 。

那么则有：$a_1+max\{a_2,b_1\}+b_2 < a_2+max\{a_1,b_2\}+b_1$ ，

再移项：$max\{a_2,b_1\}-b_1-a_2 < max\{a_1,b_2\}-b_2-a_1$ ，

化简可得：==$min\{a_1,b_2\} < min\{a_2,b_1\}$== ，这就是我们排序使用的规则。

根据上面的规则进行排序，我们一定能得到最优的解。

我们可以用反证法证明一下：

假设最优解中存在两个物品 $i$ 和 $j$ ，不满足上述规则。

我们可以交换 $i$ 和 $j$ ，这样得到的序列总时间一定会降低，那么就与最优解的假设相矛盾，我们又构造出了一个更优解。

这就说明，按上述排序规则进行排序，得到的一定是最优解。

#### 例题

[点此跳转](https://vjudge.net/problem/LibreOJ-10003)

#### AC代码

```cpp
// #pragma GCC optimize(3)
#include <set>
#include <map>
#include <cmath>
#include <stack>
#include <queue>
#include <deque>
#include <string>
#include <cstdio>
#include <bitset>
#include <vector>
#include <cstdlib>
#include <cstring>
#include <sstream>
#include <iostream>
#include <algorithm>
// #include <unordered_set>
// #include <unordered_map>
#define endl '\n'
#define x first
#define y second
#define fi first
#define se second
#define PI acos(-1)
// #define PI 3.1415926
#define LL long long
#define INF 0x3f3f3f3f
#define lowbit(x) (-x&x)
#define PII pair<int, int>
#define ULL unsigned long long
#define PIL pair<int, long long>
#define all(x) x.begin(), x.end()
#define mem(a, b) memset(a, b, sizeof a)
#define rev(x) reverse(x.begin(), x.end())
#define IOS ios::sync_with_stdio(false),cin.tie(0),cout.tie(0)

using namespace std;

const int N = 1010;

struct Data {
	int a, b, c;
	
	bool operator< (const Data& t) const {
		return min(a, t.b) < min(t.a, b);
	}
}item[N];
int n;

void solve() {
	cin >> n;
	for (int i = 0; i < n; i ++ ) cin >> item[i].a;
	for (int i = 0; i < n; i ++ ) cin >> item[i].b;
	for (int i = 0; i < n; i ++ ) item[i].c = i + 1;
	
	sort(item, item + n);
	
	int ta = 0, tb = 0;
	for (int i = 0; i < n; i ++ ) {
		ta += item[i].a;
		tb = max(ta, tb) + item[i].b;
	}
	
	cout << tb << endl;
	for (int i = 0; i < n; i ++ ) cout << item[i].c << ' ';
	cout << endl;
}

int main() {
	IOS;
	
	solve();
	
	return 0;
}
```

