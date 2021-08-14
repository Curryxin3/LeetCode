## 669. 修剪二叉搜索树
> **知识点：二叉树；递归**
### 题目描述

给你二叉搜索树的根节点 root ，同时给定最小边界low 和最大边界 high。通过修剪二叉搜索树，使得所有节点的值在[low, high]中。修剪树不应该改变保留在树中的元素的相对结构（即，如果没有被移除，原有的父代子代关系都应当保留）。 可以证明，存在唯一的答案。

所以结果应当返回修剪好的二叉搜索树的新的根节点。注意，根节点可能会根据给定的边界发生改变。

##### 示例
![image](https://note.youdao.com/yws/public/resource/6fa0eca998f3cbca5812e4ebbe017e5e/xmlnote/7673DB66FA454EDDAB09142571CA51F8/11504)
```
输入：root = [1,0,2], low = 1, high = 2
输出：[1,null,2]

输入：root = [3,0,4,null,2,null,null,1], low = 1, high = 3
输出：[3,2,null,1]

输入：root = [1], low = 1, high = 2
输出：[1]

输入：root = [1,null,2], low = 1, high = 3
输出：[1,null,2]

输入：root = [1,null,2], low = 2, high = 4
输出：[2]
```
---
### 解法一：递归
函数功能：修剪二叉树，使所有树节点都在一个范围内  
1.终止条件：root == null，返回null；  
2.该做什么：判断当前节点的值，如果当前节点< val,那整个左子树肯定都小，不用管了，去修剪右子树；如果当前节点> val，那整个右子树肯定都大，不用管了，去修剪左子树，经过这两个判断后其实如果不满足的话就已经剔除了。如果在两者之间，说明当前节点不越界，再去修剪左右子树。    
3.什么时候做：需要判断每一个节点是否越界，所以从上往下，前序。

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
    public TreeNode trimBST(TreeNode root, int low, int high) {
        if(root == null) return null;
        if(root.val > high) return trimBST(root.left, low, high); //右树肯定更大了，不用管了；
        if(root.val < low) return trimBST(root.right, low, high); //左树肯定更小了，不用管了；
        root.left = trimBST(root.left, low, high); //root满足，接着去修剪左树，并接在root上；
        root.right = trimBST(root.right, low, high);
        return root;
    }
}
```
空间复杂度：O(N):堆栈的支出
