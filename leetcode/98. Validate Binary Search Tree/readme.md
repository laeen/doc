# [98. Validate Binary Search Tree](https://leetcode.com/problems/validate-binary-search-tree/)

## Description
Given the root of a binary tree, determine if it is a valid binary search tree (BST).

A valid BST is defined as follows:

The left subtree of a node contains only nodes with keys less than the node's key.
The right subtree of a node contains only nodes with keys greater than the node's key.
Both the left and right subtrees must also be binary search trees.

**Example 1**

```
Input: root = [2,1,3]
Output: true
```

**Example 2**
```
Input: root = [5,1,4,null,null,3,6]
Output: false
Explanation: The root node's value is 5 but its right child's value is 4.
```

**Constraints:**

- The number of nodes in the tree is in the range `[1, 104]`.
- `-2^31 <= Node.val <= 2^31 - 1`

## Solution

核心思想是利用深度优先遍历（DFS)   `O(n)`

和   [94. Binary Tree Inorder Traversal](https://leetcode.com/problems/binary-tree-inorder-traversal/) 类似。


BST （二叉搜索树） 就是中序遍历后，得到的序列是一个升序的序列，那么我们就称这个数是BST， 这一题的关键在于

- **左子树的所有节点 < 根节点 < 右子树的所有节点**

两种方法解决：

- 通过中序遍历，保存好所有的输出，最后判断这个序列是不是升序（空间O(n) 时间O(n)
- 递归, 判断左子树小于当前根节点小于右子树(空间O(1) 时间O(n))




给定二叉树:

         5
        / \
       3   8
      / \
     1   6

这样的情况。 

### 方法1： 
    中序遍历得到序列  `[1, 6, 3, 5, 8]`  不满足升序，false


### 方法2：


      1.遍历 `5` ,  3 < 5 < 8  ok

      2.遍历 `3` ,  1 < 3 < 6  ok  
      3.遍历到 1  ok
      4.遍历到6,  5 < 6 不符合BST的要求

那么怎么将 `5` 这个信息传递，可以传参，遍历到 `6` 的时候, 将这个数的有边界传给它， bleft=3, bright=5, 那么就有判断  6 > 3 && 6 < 5

所以可以总结出

        1. root->left->val < root->val < root->right->val
        2. bleft < root->val < bright

关键在于 第2点， 边界。 答案给出要用long long 是因为有 *INT_MAX* 导致越界

## EN

The core idea is to use depth-first traversal (DFS) `O(n)`

Similar to [94. Binary Tree Inorder Traversal](https://leetcode.com/problems/binary-tree-inorder-traversal/).


BST (Binary Search Tree) is that after in-order traversal, the obtained sequence is an ascending sequence, then we call this number BST. The key to this question is

- **all nodes of left subtree < root node < all nodes of right subtree**

Two ways to solve:

- Through in-order traversal, save all outputs, and finally determine whether the sequence is in ascending order (space O(n) time O(n)
- Recursion, judge that the left subtree is smaller than the current root node is smaller than the right subtree (space O(1) time O(n))




Given a binary tree:

         5
        / \
       3 8
      / \
     1 6

such a situation.

### method 1: 
    The sequence `[1, 6, 3, 5, 8]` obtained by in-order traversal does not satisfy the ascending order, false


### Method 2:


      1. Traverse `5` , 3 < 5 < 8 ok

      2. Traverse `3` , 1 < 3 < 6 ok
      3. Traverse to 1 ok
      4. Traverse to 6, 5 < 6 does not meet the requirements of BST

Then how to pass the information of `5`, you can pass parameters, when traversing to `6`, pass the boundary of this number to it, bleft=3, bright=5, then there is a judgment 6 > 3 && 6 < 5

So it can be concluded that

        1. root->left->val < root->val < root->right->val
        2. bleft < root->val < bright

The key lies in point 2, the boundary. The answer is given to use long long because *INT_MAX* leads to out-of-bounds


```c++
class Solution {
public:
    bool isValidBST(TreeNode* root) {
        return helper(root, 0x8000000000000000, 0x7fffffffffffffff);
    }
    bool helper(TreeNode* root, long long left_val, long long right_val){
        if (!root) return true;
        return root->val > left_val && root->val < right_val && \
            helper(root->left, left_val, root->val) && \
            helper(root->right, root->val, right_val);
    }
};
```

