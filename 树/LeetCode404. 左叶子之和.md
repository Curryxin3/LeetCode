## 404. 左叶子之和
> **知识点：二叉树**
### 题目描述

计算给定二叉树的所有左叶子之和。。

##### 示例
```
    3
   / \
  9  20
    /  \
   15   7

在这个二叉树中，有两个左叶子，分别是 9 和 15，所以返回 24
```
---
### 解法一：DFS

函数功能：左叶子之和     
1.终止条件：root为空，返回0；     
2.能做什么：判断自己的左节点是否为空，不为空的话判断它是不是叶子节点，是的话就加到sum上；不是的话那就接着去看子树；      
3.什么时候做：从上到下，先弄自己的，再去弄子树的，前序；

做这类二叉树的题目，多半是遍历树，遍历的过程中进行一些计算。   
我们可以先序遍历这个树，遍历的同时判断当前点是否有左孩子，如果有左孩子，那左孩子是不是叶子节点，都满足的话那就是左叶子节点，累加。然后直到遍历结束；
```
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    int sum = 0;
    public int sumOfLeftLeaves(TreeNode root) {
        if(root == null) return 0;
        if(root.left != null && root.left.left == null && root.left.right == null){
            sum += root.left.val;
        }
        sumOfLeftLeaves(root.left);
        sumOfLeftLeaves(root.right);
        return sum;
    }
}
```

