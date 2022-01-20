# [62. Unique Paths](https://leetcode.com/problems/unique-paths/)

## Description
There is a robot on an `m x n` grid. The robot is initially located at the top-left corner (i.e., `grid[0][0]`). The robot tries to move to the bottom-right corner (i.e., `grid[m - 1][n - 1]`). The robot can only move either down or right at any point in time.

Given the two integers m and n, return the number of possible unique paths that the robot can take to reach the bottom-right corner.

The test cases are generated so that the answer will be less than or equal to `2 * 109`.

**Example 1**
```
Input: m = 3, n = 2
Output: 3
```

**Example 2**
```
Input: m = 3, n = 7
Output: 28
```

**Constraints:**

- `1 <= m, n <= 100`


## Solution

动态规划入门题。

## 1 分解子问题
首先，要学会判断 *是否使用动态规划* 的重要条件就是看当前的问题是否能分解成子问题(类似于递归)。


在这个题目可以提取出有用的信息

1. 每次能走一步，且只能往右或者往下。
2. 需要计算到右下角一共有多少条路线。

那么第一步，就是分解子问题。现在假设有一个 `3*7` 的矩阵， 我们利用 `A[1][1]` 表示左上角（起点）
假设现在已经走到了右下角`A[3][7]`，那么我的 ** 上一步 ** 是从哪里走过来的。

只能是从 `A[3][6]` 或者是 `A[2][7]` 走的。



## 2 状态转移方程


这个时候，问题就转换了，变成了两个子问题， 一个是求到 `A[2][7]` 的路线，另外一个是求到 `A[3][6]`

那么可以有公式：


                 A[3][7] = A[3][6] + A[2][7]


这表示到 `(3,7)` 坐标的路线总数是由  `(3,6)` 与 `(2,7)`的和

一般地，可以有

                 A[n][m] = A[n-1][m] + A[n][m-1]

            
这个方程我们称为状态转移方程

## 3 找初始状态

得到状态转移方程以后，需要找到最初始的状态。一般是通过观察状态转移方程来求。比如这一题，最边界就是第一行与第一列，也就是

         
         A[i][0] = 1
         A[0][j] = 1

```c++
class Solution {
public:
    int uniquePaths(int m, int n) {
         vector< vector<int> > a(m,vector<int>(n,0));
        
         for(int i = 0 ; i < m ; i++)a[i][0] = 1;
         for(int i = 0 ; i < n ; i++)a[0][i] = 1;

         for(int i = 1 ; i < m ; i++)
            for(int j = 1; j < n ; j++)
                a[i][j] = a[i-1][j] + a[i][j-1];
        
     return a[m-1][n-1];
    }
};
```







