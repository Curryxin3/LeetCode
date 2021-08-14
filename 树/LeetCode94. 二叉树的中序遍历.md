## 94. 二叉树的中序遍历
> **知识点：二叉树；递归；Morris遍历**
### 题目描述

给定一个二叉树的根节点 root ，返回它的 中序 遍历。
##### 示例
![image](https://note.youdao.com/yws/public/resource/6fa0eca998f3cbca5812e4ebbe017e5e/xmlnote/0FC313F9D9C04D8180B81DE6809532B9/8352)
```
输入：root = [1,null,2,3]
输出：[1,3,2]

输入：root = []
输出：[]

输入：root = [1]
输出：[1]

输入：root = [1,2]
输出：[2,1]

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
    public List<Integer> inorderTraversal(TreeNode root) {
        if(root == null) return list;
        inorderTraversal(root.left);
        list.add(root.val);
        inorderTraversal(root.right);
        return list;
    }
}
```
时间复杂度：0(N),每个节点恰好被遍历一次；   
空间复杂度：O(N),递归过程中栈的开销；
### 解法二:迭代法
1.整条左边界依次入栈；   
2.条件1执行不了了，弹出就打印；   
3.来到弹出节点的右节点，继续执行1；
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
    public List<Integer> inorderTraversal(TreeNode root) {
        Stack<TreeNode> stack = new Stack<>();
        while(!stack.isEmpty() || root != null){
            if(root != null){
                stack.push(root);  //左边一路走到头；
                root = root.left;
            }else{
                TreeNode top = stack.pop();
                list.add(top.val);
                root = top.right;
            }
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
    public List<Integer> inorderTraversal(TreeNode root) {
        TreeNode cur = root;
        TreeNode mostRightNode = null;
        while(cur != null){
            mostRightNode = cur.left;
            if(mostRightNode != null){
                while(mostRightNode.right != null && mostRightNode.right != cur){
                    mostRightNode = mostRightNode.right;
                }
                if(mostRightNode.right == null){
                    mostRightNode.right = cur;
                    cur = cur.left;
                    continue;
                }else{
                    mostRightNode.right = null;
                    list.add(cur.val); //第二次到的时候打印；
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
中序遍历的特殊在于其迭代法；

### 相关链接
[二叉树](https://www.cnblogs.com/Curryxin/p/15063475.html)
