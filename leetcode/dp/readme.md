# DP专题

## [55. Jump Game](https://leetcode.com/problems/jump-game/submissions/)

https://leetcode.com/problems/jump-game/submissions/

这个题目目的时再来巩固一下这种走楼梯式的动态规划

```c++
class Solution {
public:
    bool canJump(vector<int>& nums) {
        //问题：能不能走到最后一格，每个数字代表当前最大步长
        
        // 先找通项公式, 假设 dp[i] 表示第 i 个点最远能走到哪个点
        // dp[i] = max(nums[i] + i, dp[i-1])
        // 那么只需要证明 
        // dp[i] >= len(nums)-1
        // 那么，当 dp[i-1] < i 时，证明i点是不可到达的
        
//         int n = nums.size();
//         if(n < 2) return true;
//         vector<int> dp(n, 0);
        
//         // 初始项
//         dp[0] = nums[0] + 0;
        
//         for(int i = 1 ; i < n ; i++){
//             if(dp[i-1] < i) break;
//             dp[i] = max(nums[i] + i, dp[i-1]);
//         }     
//         return dp[n-1] >= n-1;
               
        // 化简：
        // 从通项公式我们知道，其实不需要 i-1 的项，只需要最大距离代替 dp[i-1], dp[i] 和 nums[i]可完成运算
        // 那么我们用一个最大可达距离来表示 
        // max_distance  = dp[i-1] = 
        // max_distance  < i 时，就不可到达

        
        int max_distance = 0;
        
        for(int i = 0; i < nums.size(); ++i){
            if(max_distance >= i) {
                max_distance = max(nums[i] + i, max_distance);
            }
        }
        return max_distance >= nums.size()-1;
    }
};
```

