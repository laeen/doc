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




## [1](123)

```c++
class Solution {
public:
    bool isMatch(string s, string p) {
        // 加油！这个题目很有难度，是一个模式匹配的dp问题
        // 对于s，仅包含了[a-z], 对于p, 不仅包含了[a-z], 还包含了*以及.
        
        //我们假设dp[i][j] 表示s 的第 i 个字符 匹配 p 的第 j 个字符的结果， 
        // 这里下标从1开始算哦，这样可以表示 dp[0][0] = true, 因为两个字符串都是空也是true
        
        //可以知道 当 dp[i-1][j-1] 为true的时候， dp[i][j]才能为 true 
        
        // p我们来讨论有3种情况，s只有一种情况
        // 
        // 1.当 p[j] == [a-z] 时， 是不是只需要判断 p[j] == s[i] 就能判断这两者是否相等
        // 2.当 p[j] == . 时， 是不是 p[j] == s[i] 永远为真
        /* 3.当 p[j] == * 时，这个情况就比较复杂了，但是不要慌，可以写一个样例 比如 p= a*
        是不是 * 代表前边的 a 出现的次数， 那么我们这个时候需要判断 p[j-1]是啥，
        因为题目说了，保证 * 面前会有一个字符出现.
        那根据 * 表示前一个字符出现的次数，可以分为 0 次 和 大于0次两种情况
        
            0次: 
            
                看dp[i][j-2]，比如 s=ab, p=abc*
                
                dp[2][4] = dp[2][4-2]  因为按照0来计数

                总结下来就是  dp[i][j-2]               
            n次：
                看dp[i-1][j], 表示之前的为真，类似于skip，比如s[1]和p[0]比较，s[2]也是跟p[0]比较，所以还需要看它前一次比较的结果是否正确，比如 s=aaa, p=a*
                
                这个时候有可能 p[j-2] = '.'
                
                总结下来就是 （s[i-1] == p[j-2] || p[j-2] == '.'） and dp[i-1][j]

    这个时候就可以写出状态转移方程了:

        当 p[i] != '*'
                dp[i][j] = dp[i-1][j-1] && (s[i-1] == p[j-1] || p[j-1] == '.')
        当 p[i] == '*'
                dp[i][j] = (1.等于0的情况) || (2.(大于0的情况) && dp[i-1][j-1]))
                1. dp[i][j] = dp[i][j-2] 
                2. dp[i][j] = (s[i-1] == p[j-2] || p[j-2] == '.') && dp[i-1][j]
    
    状态转移方程完成后，就是初始状态了，在转移方程里，出现了 j-2 和 i-1的项
        1.那么确认的是, 当 * 存在时， 必须 j >= 2 才行， 也就是至少是p的第二个字符
        2.dp[0][0] 表示两个串都为空，匹配成功，true
        3.dp[0][..] 表示 字符串s 为空， p长度大于0, 那么这个时候肯定是需要存在 * 才能为true
            dp[0][..] = j > 2 && dp[0][j-2] && p[j-1] == '*' 

        4.dp[..][0] 表示p为空，匹配失败，为false
        
        */
        size_t n = s.size(), m = p.size();
        
        vector< vector<bool> > dp(n+1, vector<bool>(m+1, false));
        // init dp[0][j]
        dp[0][0] = true;
        for (int j = 1 ; j <= m ; ++j){
                dp[0][j] = p[j-1] == '*' && j > 1 && dp[0][j-2];
        }
        
        for(int i = 1 ; i <= n; ++i){
            for(int j = 1 ; j <= m ; ++j){
                if(p[j-1] != '*'){
                    dp[i][j] = dp[i-1][j-1] && (s[i-1] == p[j-1] || p[j-1] == '.');
                }else{
                    dp[i][j] = j > 1 && dp[i][j-2];
                    dp[i][j] = dp[i][j] || (j>1 && ((p[j-2] == s[i-1] || p[j-2] == '.')) && dp[i-1][j]);
                }
                
            }
        }
    }
};

```


## [44. Wildcard Matching](https://leetcode.com/problems/wildcard-matching/)

这个题目是上一个题目的削弱版

```c++
class Solution {
public:
    bool isMatch(string s, string p) {
        /* 跟上一题类似，这题也是模式匹配
        p 有3种情况：[a-z], ?, *
        
        dp[i][j] 表示 s 的前 i 个字符 和 p 的前 j 个字符匹配结果
        
        1. 当 p[j-1] = ? or p[j-1] = [a-z]
            dp[i][j] = dp[i-1][j-1] && (p[j-1] == s[i-1] || p[j-1] == ?)
        
        2. 当 p[j-1] = *，
            A. 当表示空的时候,只需要看p[j-2]的时候，是不是为true 比如 s=aa   p=aa*
            dp[i][j] = dp[i][j-1]
            
            B. 当不表示为空的时候，只需要看，s[i-2] 是不是true，比如 s=aabc p=aa*
            dp[i][j] = dp[i-1][j]
            
            综上
            dp[i][j] = dp[i][j-1] || dp[i-1][j]
        
        3.初始情况， 当dp[..][0] 肯定都是false，
            d[0][..] 需要 p都是 * 才为true
            
            dp[0][j] = dp[0][j-1] && p[j-1] == '*' 
                
        */
        int n = s.size(), m = p.size();
        
	    vector< vector<int> > dp(n+1 , vector<int>(m+1 , 0));        
        
        dp[0][0] = 1;
        
        
        for(int j = 1; j <= m ; j++){
            // 这里其实出现了别的可以直接break， 但是为了便于理解，就这样写
            dp[0][j] = dp[0][j-1] && p[j-1] == '*'; 
        }
        
        for(int i = 1; i <= n; ++i){
            for(int j = 1; j <= m; ++j){
                if(p[j-1] == '*'){
                    dp[i][j] = dp[i-1][j] || dp[i][j-1];
                }else{
                    dp[i][j] = dp[i-1][j-1] && (p[j-1] == s[i-1] || p[j-1] == '?');
                }
                
            }
        }
          
        return dp[n][m];
    }
};
```