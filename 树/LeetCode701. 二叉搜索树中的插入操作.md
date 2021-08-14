## 701. 二叉搜索树中的插入操作
> **知识点：二叉树；递归**
### 题目描述

给定二叉搜索树（BST）的根节点和要插入树中的值，将值插入二叉搜索树。 返回插入后二叉搜索树的根节点。 输入数据 保证 ，新值和原始二叉搜索树中的任意节点值都不同。

注意，可能存在多种有效的插入方式，只要树在插入后仍保持为二叉搜索树即可。 你可以返回 任意有效的结果 。

##### 示例

```
给定二叉搜索树:

        4
       / \
      2   7
     / \
    1   3

和值: 5

返回：
        4
       / \
      2   7
     / \  /
    1   35
```
---
### 解法一：递归

函数功能：在树中插入节点  
1.终止条件；root==null的时候，返回一个新节点值  
2.该做什么：一个节点首先要判断自己的值和val的大小，如果val大的的话就去交给右子树处理，小的话交给左子树处理。  
3.什么时候做：在开始就得判断，才能知道往那个子树上走，前序。

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
    public TreeNode insertIntoBST(TreeNode root, int val) {
        if(root == null) return new TreeNode(val);
        //val大的时候去交个左子树，注意最后返回的是整棵树，所以要root.left= ....；  
        //注意要区别递归什么时候写return,什么时候写root.left；
        if(root.val > val) root.left = insertIntoBST(root.left, val);
        else root.right = insertIntoBST(root.right, val);
        return root;
    }
}
```
空间复杂度：O(N):堆栈的支出
### 解法二：迭代
二叉搜索树的性质：对于任意节点root 而言，左子树（如果存在）上所有节点的值均小于root.val，右子树（如果存在）上所有节点的值均大于root.val，且它们都是二叉搜索树。    
- 如果子树不为空，那就将值插入到对应子树上，
- 如果子树为空，那就新建一个节点，并连接到父节点上。

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
    public TreeNode insertIntoBST(TreeNode root, int val) {
        if(root == null) return new TreeNode(val);
        TreeNode cur = root;
        while(cur != null){
            if(cur.val > val){
                if(cur.left == null) {
                    cur.left = new TreeNode(val);
                    break;
                }
                else cur = cur.left;
            }else{
                if(cur.right == null) {
                    cur.right = new TreeNode(val);
                    break;
                }
                else cur = cur.right;
            }
        }
        return root;
    }
}
```
时间复杂度：
