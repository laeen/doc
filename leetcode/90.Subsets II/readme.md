# [90. Subset II](https://leetcode.com/problems/subsets-ii/)

## Description
Given an integer array nums that may contain duplicates, return all possible subsets (the power set).

The solution set must not contain duplicate subsets. Return the solution in any order.

**Example 1**
```
Input: nums = [1,2,2]
Output: [[],[1],[1,2],[1,2,2],[2],[2,2]]
```

**Example 2**
```
Input: nums = [0]
Output: [[],[0]]
```

**Constraints:**

- 1 <= nums.length <= 10
- -10 <= nums[i] <= 10

## Solution

核心思想是利用深度优先遍历（DFS)   `O(2^n)`

和   [78. Subset](https://leetcode.com/problems/subsets/)  类似， 但是这里不一样的地方在于数字可以是重复的，这就需要过滤掉重复的集合。

有两种方法可以：

1. 使用 `78.subsets` 的解法，然后在最后过滤一下
2. 在计算时直接过滤掉重复的集合

对于 `方法1`， 可能会产生较大的内存，因为会存储不必要的集合，比如`[1,1,1]` 会产生 `[1]`, `[1,1]`, `[1,1,1]` 3个集合，但是采用之前的方式会产生 6 个集合， 产生的差为

                  3！- 3 = 3

一般地，这个可以作为推广，如果10个数字都相同，那么是      

                 10！- 10 ≈ 10！

`方法1` 舍弃。只能采用 `方法2`

**排除相同数字的集合** 就可以达到过滤的效果

比如`[1, 1, 1]` 其实只需计算第一个  `1` 就能得到
`[1]`, `[1, 1]`, `[1, 1, 1]` 

可以得出结论， **跳过相同的数字来进行计算**





Main idea is using Depth First Search(DFS) `O(2^n)`

this problem is similar to [78. Subset](https://leetcode.com/problems/subsets/).
but array numbers may contain duplicates in this problem. so we need to distinct our output array.

there is 2 way to solve it

1. Use the solution of `78.subsets` and filter at the end
2. Directly filter out duplicate sets during calculation

For `Method 1`, larger memory may be generated because unnecessary collections will be stored. For example, `[1,1,1]` will generate `[1]`, `[1,1]`, `[ 1,1,1]` 3 sets, but using the previous method will produce 6 sets, the difference is

                   3! -3 = 3

Generally, this can be used as a promotion. If all 10 numbers are the same, then it is

                  10! -10 ≈ 10!

`Method 1` Discard. Only use `Method 2`

**Excluding the collection of the same number** can achieve the filtering effect

For example, `[1, 1, 1]` actually only needs to calculate the first `1` to get
`[1]`, `[1, 1]`, `[1, 1, 1]`

It can be concluded that **Skip the same number for calculation**


```
class Solution {
public:
    vector<vector<int>> subsetsWithDup(vector<int>& nums) {
        sort(nums.begin() , nums.end());
        vector< vector<int> >ret(1,vector<int>(0));
        int pre = INT_MIN , start = 0;
        for(int i = 0 ; i < nums.size() ; ++i){           
            int n = ret.size();
            if(i && nums[i] == nums[i-1])start = pre;
            else start = 0;         
            for(int j = start ; j < n ; ++j){
                ret.push_back(ret[j]);
                ret.back().push_back(nums[i]);
            }
            pre = n ;                              
        }
        
        return ret;
    }
};
```






