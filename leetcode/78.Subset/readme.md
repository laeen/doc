# [78. Subset](https://leetcode.com/problems/subsets/)

## Description
Given an integer array nums of unique elements, return all possible subsets (the power set).

The solution set must not contain duplicate subsets. Return the solution in any order.

**Example 1**
```
Input: nums = [1,2,3]
Output: [[],[1],[2],[1,2],[3],[1,3],[2,3],[1,2,3]]
```

**Example 2**
```
Input: nums = [0]
Output: [[],[0]]
```

**Constraints:**

- 1 <= nums.length <= 10
- -10 <= nums[i] <= 10
- All the numbers of nums are unique.


## Solution

核心思想是利用深度优先遍历（DFS)   `O(2^n)`

1. 将所有的数字当作节点。
2. 空集合是根节点，`空集`可以和任意一个节点进行连接，就有了1个元素的子集
3. 条件中有说每个数字都不相同。根据`集合`的特性（每个数字都唯一）可以得出，任意一个节点的子节点是这个节点的右边兄弟节点。

如序列[1,2,3,4]

                      []
                  /  /  \  \
                 1  2   3   4
              / / | ... .. ...
             2  3 4



Main idea is using Depth First Search(DFS) `O(2^n)`

1. we can treat all numbers as nodes
2. the empty set is root and there are `1 element` sets which root can be connect any nodes 
3. on constraints, we know that all the numbers of nums are unique, so it's easy to infer the child nodes of any node are the right sibling of this node

```
class Solution {
public:
    vector<vector<int>> subsets(vector<int>& nums) {
        vector<vector<int>> ret;
        vector<int> temp;
        dfs(nums, ret, temp, 0); // 调用第 0 层
        return ret;
    }
    
    // 相当于分解成子问题 比如 长度为 n 的序列[1,2,3,4...,n-1,n] 的问题可以简化成 
    // 长度为n-1的子问题 [2,3,4...n-1, n] 
    // 这样简化到最后就可以求解
    // 也可以理解成树。        []
    //                    /  |  \
    //                  1    2    3
    //                 / \   |
    //                2   3  3
    // 这样每个节点都能跟根节点形成一条路， 就是一个集合。 两种理解方式都可以
    void dfs(vector<int>& nums, vector<vector<int>>& ret,vector<int>& temp, int deep){
        int len = nums.size();
        ret.push_back(temp);
        if (deep >= len){
            return ;
        }
        
        for(int i = deep ; i < len; ++i){
            temp.push_back(nums[i]);
            dfs(nums, ret, temp, i+1);
            temp.pop_back();
        }
    }
};
```

**another solution**

可以利用集合相加的形式，这样做的好处是防止当数组比较大的时候，可能会引起崩溃。 `O(2^n)`

We can use set-addition. The advantage of this is to prevent crashes when the `nums` is relatively large


```
class Solution {
public:
    vector<vector<int>> subsets(vector<int>& nums) {
        vector<vector<int>>ret(1,vector<int>(0));
        for(int val : nums){
            int n = ret.size();
            for(int i = 0 ; i < n ; ++i){
                ret.push_back(ret[i]);
                ret.back().push_back(val);
            }
        }
        
        return ret;
    }
};
```








