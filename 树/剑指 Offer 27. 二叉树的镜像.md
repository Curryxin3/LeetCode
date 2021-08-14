## 剑指 Offer 27. 二叉树的镜像
> **知识点：二叉树，递归**
### 题目描述

请完成一个函数，输入一个二叉树，该函数输出它的镜像。

![image](https://note.youdao.com/yws/public/resource/6fa0eca998f3cbca5812e4ebbe017e5e/xmlnote/47116C133C0447AB8567955DAB8E27BC/9033)

##### 示例
```
输入：root = [4,2,7,1,3,6,9]
输出：[4,7,2,9,6,3,1]
```
---
### 解法一：递归法
函数功能：二叉树镜像；
1、终止条件：root==null的时候，返回null；
2、每个节点该做什么：将两个孩子节点互换；
3、什么时候做：递去的时候或者回来的时候都行（前序或者后序都行）
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
    public TreeNode mirrorTree(TreeNode root) {
        if(root == null) return null;
        TreeNode temp = root.right;
        root.right = root.left;
        root.left = temp;
        mirrorTree(root.left);
        mirrorTree(root.right);
        return root;
    }
}
```
时间复杂度；0(N),每个节点恰好被遍历一次；   
空间复杂度；O(N),递归过程中栈的开销；
### 解法二：栈
自己构造一个辅助栈，依次遍历所有的节点，并交换每个节点的左右孩子；  
**规则：**  
压入根节点；  
1.弹出；  
2.压入弹出节点的左右孩子，并交换；   
其实是一层一层，一个节点一个节点的处理。  
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
    public TreeNode mirrorTree(TreeNode root) {
        if(root == null) return null;
        Stack<TreeNode> stack = new Stack<>();
        stack.push(root);
        while(!stack.isEmpty()){
            TreeNode top = stack.pop();
            if(top.left != null) stack.push(top.left);
            if(top.right != null) stack.push(top.right);
            TreeNode temp = top.left;
            top.left = top.right;
            top.right = temp;
        }
        return root;
    }
}
```
时间复杂度；0(N),每个节点恰好被遍历一次；   
空间复杂度；O(N),递归过程中栈的开销

### 相关链接
[图解二叉树的镜像](https://leetcode-cn.com/problems/er-cha-shu-de-jing-xiang-lcof/solution/mian-shi-ti-27-er-cha-shu-de-jing-xiang-di-gui-fu-/)
