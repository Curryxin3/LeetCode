## 106. 从中序与后序遍历序列构造二叉树
> **知识点：二叉树，递归**
### 题目描述

根据一棵树的中序遍历与后序遍历构造二叉树。

注意:
你可以假设树中没有重复的元素。

##### 示例
```
中序遍历 inorder = [9,3,15,20,7]
后序遍历 postorder = [9,15,7,20,3]

返回如下的二叉树：

    3
   / \
  9  20
    /  \
   15   7
```
---
### 解法一：递归法
此题和105题基本一样，所以我们仍然使用这样的思路继续下去。
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
    public TreeNode buildTree(int[] inorder, int[] postorder) {
        if(postorder == null) return null;
        Map<Integer, Integer> map = new HashMap<>();
        for(int i = 0; i < inorder.length; i++){
            map.put(inorder[i], i);
        }
        return buildTree(inorder, postorder, 0, inorder.length-1, 0, postorder.length-1, map);
    }
    private TreeNode buildTree(int[] inorder, int[] postorder, int inleft, int inright, int postleft, int postright, Map<Integer,Integer> map){
        if(postleft > postright) return null;
        TreeNode root = new TreeNode(postorder[postright]);
        int rootIndex = map.get(root.val);
        int leftTreeSize = rootIndex-inleft;
        root.left = buildTree(inorder, postorder, inleft, rootIndex-1, postleft, postleft+leftTreeSize-1,map);
        root.right = buildTree(inorder, postorder, rootIndex+1, inright, postleft+leftTreeSize, postright-1,map);
        return root;
    }
}
```
