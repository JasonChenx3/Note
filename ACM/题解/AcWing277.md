[题目链接](https://www.acwing.com/problem/content/279/)

#### 算法

动态规划。

#### 分析

根据排序不等式。

先将 $g$ 数组降序排序，排名靠前的小朋友分到的饼干更多。

![原理图](https://cdn.acwing.com/media/article/image/2019/10/12/1_b0629426ec-acwing_277.png)

最后倒推出方案。

#### 状态定义

集合：`f[i][j]` 分给前 $i$ 个孩子 $j$ 个饼干的所有方案。

属性：最小值。

#### 状态计算

考虑最后有几个小朋友分到 $1$ 个饼干。

假设最后连续有 $k$ 个小朋友分到 $1$ 个饼干。

$k = 0$ 时：

- `f[i][j] = f[i][j - i]`，`f[i][j]` 的最小值和 `f[i][j - i]` 的最小值相同，因为所有小朋友的饼干数大于 $0$ 。

$k \ne 0$ 时：

- `f[i][j] = f[i][j - k] + (sum[i] - sum[i - k]) * (i - k)` 

![动态规划分析图](https://cdn.acwing.com/media/article/image/2019/10/12/1_ca93c968ec-acwing_277_2.png)