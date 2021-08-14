## 144. 二叉树的前序遍历
> **知识点：二叉树；递归；Morris遍历**
### 题目描述

给你二叉树的根节点 root ，返回它节点值的 前序 遍历。
##### 示例
![image](https://note.youdao.com/yws/public/resource/6fa0eca998f3cbca5812e4ebbe017e5e/xmlnote/0FC313F9D9C04D8180B81DE6809532B9/8352)
```
输入：root = [1,null,2,3]
输出：[1,2,3]

输入：root = []
输出：[]

输入：root = [1]
输出：[1]

输入：root = [1,2]
输出：[1,2]

输入：root = [1,null,2]
输出：[1,2]
```
---
### 解法一：递归

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
    List<Integer> list = new ArrayList<>();
    public List<Integer> preorderTraversal(TreeNode root) {
        if(root == null) return list;
        list.add(root.val);
        preorderTraversal(root.left);
        preorderTraversal(root.right);
        return list;
    }
}
```
时间复杂度；0(N),每个节点恰好被遍历一次；   
空间复杂度；O(N),递归过程中栈的开销；
### 解法二:迭代法
入栈一定是先右后左，这样出来才能使先左后右；
压入根节点；    
1.弹出就打印；  
2.如有右孩子，压入右；  
3.如有左孩子，压入左；  
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
    public List<Integer> preorderTraversal(TreeNode root) {
        Stack<TreeNode> stack = new Stack<>();
        List<Integer> list = new ArrayList<>();
        if(root != null) stack.push(root);
        while(!stack.isEmpty()){
            TreeNode top = stack.pop();
            list.add(top.val);
            if(top.right != null) stack.push(top.right);
            if(top.left != null) stack.push(top.left);
        }
        return list;
    }
}
```
### 解法三：Morris遍历
构建从下到上的连接，一条路能够走遍所有节点；
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
    List<Integer> list = new ArrayList<>();
    public List<Integer> preorderTraversal(TreeNode root) {
        if(root == null) return list;
        TreeNode cur = root;
        TreeNode mostRightNode = null;
        while(cur != null){
            mostRightNode = cur.left;
            if(mostRightNode != null){
                //有左子树；
                while(mostRightNode.right != null && mostRightNode.right != cur){
                    mostRightNode = mostRightNode.right; //找到左子树的最右节点；
                }
                if(mostRightNode.right == null){
                    mostRightNode.right = cur; //构建向上的连接；
                    list.add(cur.val);
                    cur = cur.left;
                    continue;
                }else{
                    //第二次到节点，断开连接
                    mostRightNode.right = null;
                    cur = cur.right;
                }
            }else{
                list.add(cur.val);
                cur = cur.right;
            }
        }
        return list;
    }
}
```
### 体会
二叉树的遍历是二叉树的最基础的，不仅要掌握递归写法，迭代法和morris遍历也需要掌握，更多详细可参考这篇总结[二叉树](https://www.cnblogs.com/Curryxin/p/15063475.html)

### 相关题目
[94. 二叉树的中序遍历](https://www.cnblogs.com/Curryxin/p/15064881.html)     
[145. 二叉树的后序遍历](https://www.cnblogs.com/Curryxin/p/15065133.html)

### 相关链接
[二叉树](https://www.cnblogs.com/Curryxin/p/15063475.html)
