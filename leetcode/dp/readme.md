# DP专题

## [55. Jump Game](https://leetcode.com/problems/jump-game/submissions/)

https://leetcode.com/problems/jump-game/

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

## [45. Jump Game II](https://leetcode.com/problems/jump-game-ii/)

https://leetcode.com/problems/jump-game-ii/

最小步数问题，这种问题的关键是，不仅仅只有一个dp[i]来参与计算

```c++
class Solution {
public:
    int jump(vector<int>& nums) {
        
        // 这题不一样的是增加了步数的计算, 要最小的步数到达
        // 我们使用 dp[i] 表示 走到 i 需要的最短步长
        
        // 这个时候，dp[i] 存在两种情况，
        // 1.nums[i] 的点，扩张了 x 最大距离,  k1表示之前的最大距离，k2表示经过了nums[i]点扩张后的距离
        
        // |-------------------dp----------------|---x----|
        // 0.... -----------------------------i--k1------k2
        
        // 2.nums[i] 的点，未扩张最大距离  dp[i] = dp[i-1]
        
        
        
        int n = nums.size();
        // dp.size() 表示当前的max_distance, (这里题目有个小坑，原来在原点是算做0)
        vector<int> dp(1, 0);
                
        for(int i = 0; i < n; ++i){
            int distance = i + nums[i] + 1;
            int step = dp[i]+1;

            // 当前距离未超过 max_distance
            for(int k = dp.size(); k < distance && k < n; ++k){
                dp.push_back(step);  
            }
        }
        return dp[n-1];
    }
};
```


## [322. Coin Change](https://leetcode.com/problems/coin-change/)

https://leetcode.com/problems/coin-change/

01 背包问题， 0 1背包表示每种东西都有两种状态
0：表示不拿
1：表示拿

这个题目时衍生问题，每个硬币有无限多

```c++
class Solution {
public:
    int coinChange(vector<int>& coins, int amount) {
        // 最经典的01 背包问题，这个是衍生版本，不过也差不多
        // 这种题就是假设 dp[i] 表示i块钱需要的dp[i]个硬币。
        // 这里假设当现在需要i块钱时，有两种情况
        //  
        //状态转移方程就是
        // 1.当需要将coins[k]放入口袋，dp[i] = dp[i-coins[k]] + 1 (k=0,1,2,3...n), i>=coins[k]
        // 2.不需要将coins[k]放入口袋，dp[i] = dp[i]
        // 取这两者的最小值
        // 然后初始状态就是dp[0] = 0, 其他的就设置成一个大一点的数字，题目是10^4, 可以直接设成amount + 1 (不会存在小于1的硬币)
        
        // vector<int> dp(n,x), 表示初始化一个 长度为n， 初始值为x的vector
        
        vector<int> dp(amount+1, amount+1);
        
        dp[0] = 0;
        
        for(int i = 1; i <= amount; ++i){
            for(int k = 0; k < coins.size(); ++k){
                //dp[i] = dp[i-coins[k]] + 1 (k=0,1,2,3...n), i>=coins[k]
                if(coins[k] <= i)
                    dp[i] = min(dp[i-coins[k]] + 1, dp[i]);
                
            }
            
        }
    return dp[amount] == amount+1 ? -1 : dp[amount];
    }
};
```