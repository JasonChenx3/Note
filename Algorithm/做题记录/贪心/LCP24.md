# [LCP 24. 数字游戏](https://leetcode.cn/problems/5TxKeK/description/)

```cpp
/**
中位数贪心。
 */
class Solution {
public:
    vector<int> numsGame(vector<int>& nums) {
        const int MOD = 1E9 + 7;
        int n = nums.size();
        vector<int> ans(n);
        priority_queue<int> left;
        priority_queue<int, vector<int>, greater<int>> right;
        long long left_sum = 0, right_sum = 0;
        for (int i = 0; i < n; i ++ ) {
            int b = nums[i] - i;
            if (i % 2 == 0) {
                if (!left.empty() && b < left.top()) {
                    left_sum -= left.top() - b;
                    left.push(b);
                    b = left.top();
                    left.pop();
                }
                right_sum += b;
                right.push(b);
                ans[i] = (right_sum - right.top() - left_sum) % MOD;
            } else {
                if (b > right.top()) {
                    right_sum += b - right.top();
                    right.push(b);
                    b = right.top();
                    right.pop();
                }
                left_sum += b;
                left.push(b);
                ans[i] = (right_sum - left_sum) % MOD;
            }
        }
        return ans;
    }
};
```
