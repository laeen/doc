# [131. Palindrome Partitioning](https://leetcode.com/problems/palindrome-partitioning/)

## Description
Given a string s, partition s such that every substring of the partition is a palindrome. Return all possible palindrome partitioning of s.

A **palindrome** string is a string that reads the same backward as forward.

**Example 1**
```
Input: s = "aab"
Output: [["a","a","b"],["aa","b"]]
```

**Example 2**
```
Input: s = "a"
Output: [["a"]]
```

**Constraints:**

- `1 <= s.length <= 16`
- `s` contains only lowercase English letters.

## Solution

核心思想是利用深度优先遍历（DFS)   `O(2^n)`

和   [78. Subset](https://leetcode.com/problems/subsets/)  类似， 但是这里不一样的地方是递归的条件，这里需要判断一下什么样的条件才能进行递归调用。

通过题目描述，要是回文子串，那么每次调用的条件就是，当前遍历的层，必须要满足是回文的条件（按照树的方式就是，每一个节点都是一个回文）

比如序列 `[1, 2, 2, 3, 4]`

        当 i = 1 时
        1. '2' 本身是一个节点可以进行一次调用。
        2. '22' 也可以构成回文，来进行一次调用。


通过这几题可以知道，递归的本质就是 *子问题的拆解*。

这类问题的变化无非就是：

- **增加递归的条件**

- **剪枝**



The core idea is to use depth-first traversal (DFS) `O(2^n)`

Similar to [78. Subset](https://leetcode.com/problems/subsets/), but the difference here is the recursive condition. It is necessary to judge what kind of condition can be used for recursive call.

According to the title description, if it is a palindrome substring, then the condition of each call is that the current traversed layer must satisfy the condition of being a palindrome (according to the way of the tree, each node is a palindrome)

For example the sequence `[1, 2, 2, 3, 4]`

         when i = 1
         1. '2' itself is a node that can make one call.
         2. '22' can also constitute a palindrome to make a call.


From these questions, we can know that the essence of recursion is *the disassembly of subproblems*.

Variations of such issues are nothing more than:

- **Increase recursion condition**

- **pruning**






