## 剑指 Offer 55 - II. 平衡二叉树
> **知识点：二叉树，递归**
### 题目描述

输入一棵二叉树的根节点，判断该树是不是平衡二叉树。如果某二叉树中任意节点的左右子树的深度相差不超过1，那么它就是一棵平衡二叉树。

##### 示例
```
给定二叉树 [3,9,20,null,null,15,7]
    3
   / \
  9  20
    /  \
   15   7
返回 true 。

给定二叉树 [1,2,2,3,3,null,null,4,4]
       1
      / \
     2   2
    / \
   3   3
  / \
 4   4
返回 false 。
```
---
### 解法一：递归(自顶向下)

函数功能：判断是是否是平衡二叉树     
1.终止条件：root为空，返回true;   
2.该做什么：怎么就能知道是不是平衡了？左树平衡，右树平衡，左右两树的深度不超过1；所以得能获得左右子树的深度，上到题做过了。      
3.什么时候做：得拿到下面的信息上面才能做，后序；     
```
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public boolean isBalanced(TreeNode root) {
        if(root == null) return true;
        return Math.abs(getDepth(root.left)-getDepth(root.right)) <= 1 && isBalanced(root.left) && isBalanced(root.right);
    }
    private int getDepth(TreeNode root){
        if(root == null) return 0;
        return Math.max(getDepth(root.left), getDepth(root.right))+1;
    }
}
```
这种解法在递归的过程中涉及到了大量的重复计算，
### 解法二：剪枝(自底向上)       
上面的解法一下子弄出来两层递归，想一下，第二个递归就是用来判断是否平衡的，而是否平衡取决于两个子树深度，所以可以从下往上，依次得到树的深度，如果小于2，就给赋值比如-1，然后每次都判断一下是否是-1就可以了。    

```
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public boolean isBalanced(TreeNode root) {
        if(root == null) return true;
        return depth(root) != -1;
    }
    private int depth(TreeNode root){
        if (node == null) return 0;
        int left = depth_1(node.left);//左边深度；
        if (left == -1) return -1; //如果不满足直接不用执行后面了，剪枝；
        int right = depth_1(node.right);
        if (right == -1) return -1;
        //如果<2:返回深度；
        //如果>2:返回-1；
        return Math.abs(left-right) < 2 ? Math.max(left,right)+1 : -1;     
    }
}
```
### 相关链接
[平衡二叉树](https://leetcode-cn.com/problems/ping-heng-er-cha-shu-lcof/solution/mian-shi-ti-55-ii-ping-heng-er-cha-shu-cong-di-zhi/)
