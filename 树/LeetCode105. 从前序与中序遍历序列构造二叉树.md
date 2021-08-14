## 105. 从前序与中序遍历序列构造二叉树
> **知识点：二叉树，递归**
### 题目描述

根据一棵树的中序遍历与前序遍历构造二叉树。

注意:
你可以假设树中没有重复的元素。

##### 示例
```
Input: preorder = [3,9,20,15,7], inorder = [9,3,15,20,7]
Output: [3,9,20,null,null,15,7]


返回如下的二叉树：

    3
   / \
  9  20
    /  \
   15   7
   
Input: preorder = [-1], inorder = [-1]
Output: [-1]
```
---
### 解法一：递归

函数的功能：从一个树的前序和中序遍历中还原一颗树。     
1.终止条件：如果前序和中序的数组为空，返回null；    
2.该做什么：根据前序遍历的特点能够知道第一个就是其根节点，然后根据此值可以在中序遍历中定位出来，然后根据在中序遍历中的值可以将其分成左右两个序列，左边就是其左子树，右边就是其右子树，这样就得到了左子树和右子树的中序遍历；同理，在前序中第一个元素后跟着的左子树个数个元素就是左子树的前序遍历，再跟着的就是右子树的前序遍历，这样就又能到了前序和中序，递归调用就可以了。      
3.什么时候做：逐步缩小树的规模，提供子树的前序和中序供下层使用：前序；       

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
    public TreeNode buildTree(int[] preorder, int[] inorder) {
        if(preorder == null) return null;
        Map<Integer, Integer> map = new HashMap<>();
        for(int i = 0; i < inorder.length; i++){
            map.put(inorder[i], i);
        }
        return buildTree(preorder, inorder, 0, preorder.length-1, 0, inorder.length-1, map);
    }
    private TreeNode buildTree(int[] preorder, int[] inorder, int preleft, int preright, int inleft, int inright, Map<Integer, Integer> map){
        if(preleft > preright) return null;
        TreeNode root = new TreeNode(preorder[preleft]);
        int rootIndex = map.get(root.val);
        int leftTreeSize = rootIndex-inleft;
        root.left = buildTree(preorder, inorder, preleft+1, preleft+leftTreeSize, inleft, rootIndex-1, map);
        root.right = buildTree(preorder, inorder, preleft+leftTreeSize+1, preright, rootIndex+1, inright, map);
        return root;
    }
}
```

### 相关题目
[106. 从中序与后序遍历序列构造二叉树](https://www.cnblogs.com/Curryxin/p/15067411.html)
