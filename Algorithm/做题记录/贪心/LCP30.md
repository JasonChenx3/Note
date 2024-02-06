# [LCP 30. 魔塔游戏](https://leetcode.cn/problems/p0NxJO/description/)

```cpp
/**
反悔贪心。
当血量小于0时，反悔一次前面所遇到的最大的负数。
 */
class Solution {
public:
    int magicTower(vector<int>& nums) {
        if (accumulate(nums.begin(), nums.end(), 0LL) <= -1) {
            return -1;
        }
        long long ans = 0, sum = 1;
        priority_queue<int> pq;
        int n = nums.size();
        for (int i = 0; i < n; i ++ ) {
            sum += nums[i];
            if (nums[i] < 0) {
                pq.push(-nums[i]);
            }
            if (sum <= 0) {
                ans ++ ;
                sum += pq.top();
                pq.pop();
            }
        }
        return ans;
    }
};
```
