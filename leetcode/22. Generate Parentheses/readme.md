# [22. Generate Parentheses](https://leetcode.com/problems/generate-parentheses/)

## Description
Given `n` pairs of parentheses, write a function to generate all combinations of well-formed parentheses.


**Example 1**
```
Input: n = 3
Output: ["((()))","(()())","(())()","()(())","()()()"]
```

**Example 2**
```
Input: n = 1
Output: ["()"]
```

**Constraints:**

- `1 <= s.length <= 8`

## Solution

核心思想是利用深度优先遍历（DFS)   `O(2^n)`

和   [94. Binary Tree Inorder Traversal](https://leetcode.com/problems/binary-tree-inorder-traversal),  [144. Binary Tree Preorder Traversal](https://leetcode.com/problems/binary-tree-preorder-traversal), [145. Binary Tree Postorder Traversal](https://leetcode.com/problems/binary-tree-postorder-traversal), 类似， 但是这里不一样的地方是递归的条件。

如何减小问题规模。
那就是n -> n-1 也就是消除了 一对 `'()'`
那就是左边减了 `1` , 右边也减了 `1`, 这样问题规模就减小了。

比如 n=2，序列 `(())`，满足要求

        1. 左边 '(' 必须出现在 ')' 之前
        2. 左右括号数量相等且等于 2 

那么左边的调用必须要在右边之前.


并且可以推导出递归结束条件:

        右边括号 = 左边括号 = N

那么递归最后的结束条件就是
```
    if(left == right && left == n){
        //do something
        return;
    }
```

这里有3个参数，可以减少`n`参数

```
    if(left == right && left == 0){
        //do something
        return;
    }
```

这样变成了减法的操作，减少了一个参数位置。

那么可以看看函数需要几个参数：

- 答案结果（vector引用），节省内存
- 当前的括号串
- 左边括号的数量
- 右边括号的数量

```
void dfs(vector<string>& ret, string t, int left, int right);
```

现在只需要调用条件， 就可以完成这道题目。

那调用条件是什么呢，要怎么去递归调用解决呢。

通过上面的 `left` 和 `right` 调用知道，必须要将这两个变量减为 `0`. 

并且有一个隐藏的条件是，右边的数量永远大于等于左边的

```
        if(left>0) dfs(ret, t + '(', left-1, right);
        if(right>0 && left < right) dfs(ret, t + ')', left, right-1);
```

The core idea is to use depth-first traversal (DFS) `O(2^n)`

and [94. Binary Tree Inorder Traversal](https://leetcode.com/problems/binary-tree-inorder-traversal), [144. Binary Tree Preorder Traversal](https://leetcode.com/problems/binary- tree-preorder-traversal), [145. Binary Tree Postorder Traversal](https://leetcode.com/problems/binary-tree-postorder-traversal), similar, but the difference here is the recursive condition.

How to reduce the problem size.
That is n -> n-1, which is to eliminate a pair of `'()'`
That is, the left side is reduced by `1`, and the right side is also reduced by `1`, so that the problem size is reduced.

For example, n=2, the sequence `(())`, meets the requirements

        1. Left '(' must appear before ')'
        2. The number of left and right parentheses is equal and equal to 2

Then the call on the left must precede the call on the right.


And the recursion end condition can be deduced:

        Right parenthesis = Left parenthesis = N

Then the final termination condition of the recursion is
````
    if(left == right && left == n){
        //do something
        return;
    }
````

There are 3 parameters here to reduce the `n` parameter

````
    if(left == right && left == 0){
        //do something
        return;
    }
````

This becomes a subtraction operation, reducing one parameter position.

Then you can see that the function takes several parameters:

- answer result (vector reference), saving memory
- the current bracketed string
- the number of left parentheses
- the number of right parentheses

````
void dfs(vector<string>& ret, string t, int left, int right);
````

Now you only need to call the condition, you can complete this problem.

What is the calling condition, and how to solve it recursively.

As you know from the `left` and `right` calls above, these two variables must be reduced to `0`.

And there is a hidden condition that the number on the right is always greater than or equal to the number on the left

````
        if(left>0) dfs(ret, t + '(', left-1, right);
        if(right>0 && left < right) dfs(ret, t + ')', left, right-1);
````

```
class Solution {
public:
    vector<string> generateParenthesis(int n) {
        vector<string> ret;
        dfs(ret, "", n, n);
        return ret;
    }
    
    void dfs(vector<string>&ret, string t, int left, int right){
        
        if(left == 0 && right == 0){
            ret.push_back(t);
            return;
        }
        
        if(left>0) dfs(ret, t + '(', left-1, right);
        if(right>0 && left < right) dfs(ret, t + ')', left, right-1);

    }
};
```



