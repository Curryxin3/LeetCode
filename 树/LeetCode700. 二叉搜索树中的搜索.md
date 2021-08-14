## 700. 二叉搜索树中的搜索
> **知识点：二叉树；递归**
### 题目描述

给定二叉搜索树（BST）的根节点和一个值。 你需要在BST中找到节点值等于给定值的节点。 返回以该节点为根的子树。 如果节点不存在，则返回 NULL。

##### 示例

```
给定二叉搜索树:

        4
       / \
      2   7
     / \
    1   3

和值: 2

返回：
      2     
     / \   
    1   3
在上述示例中，如果要找的值是   
5，但因为没有节点值为 5，我们应该返回 NULL。
```
---
### 解法一：递归   

#### 二叉搜索树
首先应该明白二叉搜索树的含义：任意一棵树的节点都大于左子树，并且小于右子树；   
尤其是二叉搜索树的根节点不仅大于左节点，是大于左子树，也就是上左树上的任意一个节点，右节点同理。并且树中的任意一个节点都是二叉搜索树；   
一个很重要的性质：**二叉搜索树按中序遍历完之后是一个递增序列；**

函数功能：找到和值相等的节点  
1.终止条件：root==null，则返回null。
2.该做什么：拿到值后判断值是否和自己一样，如果一样就返回，如果不一样，则分别看值和节点谁大然后分别去左树和右树上继续找。  
3.先判断根节点才能知道往哪个树上去，前序。
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
    public TreeNode searchBST(TreeNode root, int val) {
        if(root == null) return null;
        if(root.val == val) return root;
        else if(root.val > val) return searchBST(root.left, val);
        else return searchBST(root.right, val);
    }
}
```
空间复杂度：O(N):堆栈的支出
### 解法二：迭代
直接根据值去左树和右树上去找，直到找到或者走到头为止，可以减少空间复杂度。

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
    public TreeNode searchBST(TreeNode root, int val) {
        if(root == null) return root;
        TreeNode cur = root;
        while(cur != null){
            if(cur.val == val) return cur;
            else if(cur.val > val) cur = cur.left;
            else cur = cur.right;
        }
        return null;
    }
}
```

### 体会  

要熟练掌握二叉搜索树的递归，往往会用到当前值和给定元素的大小关系，因为二叉搜索树是有严格大小关系的，根据比较的结果去左树或者右树。   
其次要掌握二叉搜索树的关键性质：**其中序遍历是递增数列；**

### 相关题目(二叉搜索树)

[701. 二叉搜索树中的插入操作](https://www.cnblogs.com/Curryxin/p/15091536.html)  
[98. 验证二叉搜索树](https://www.cnblogs.com/Curryxin/p/15091660.html)  
[235. 二叉搜索树的最近公共祖先](https://www.cnblogs.com/Curryxin/p/15091755.html)  
[669. 修剪二叉搜索树](https://www.cnblogs.com/Curryxin/p/15091858.html)
