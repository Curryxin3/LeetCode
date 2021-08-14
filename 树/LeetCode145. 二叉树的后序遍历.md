## 145. 二叉树的后序遍历
> **知识点：二叉树；递归；Morris遍历**
### 题目描述

给定一个二叉树的根节点 root ，返回它的 后序 遍历。
##### 示例
![image](https://note.youdao.com/yws/public/resource/6fa0eca998f3cbca5812e4ebbe017e5e/xmlnote/0FC313F9D9C04D8180B81DE6809532B9/8352)
```
输入: [1,null,2,3]  
   1
    \
     2
    /
   3 

输出: [3,2,1]
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
    public List<Integer> postorderTraversal(TreeNode root) {
        if(root == null) return list;
        postorderTraversal(root.left);
        postorderTraversal(root.right);
        list.add(root.val);
        return list;
    }
}
```
时间复杂度：0(N),每个节点恰好被遍历一次；   
空间复杂度：O(N),递归过程中栈的开销；
### 解法二:迭代法
后序遍历可以用前序遍历来解决，想一下前序遍历：根左右，我们先压右树再压左树。怎么实现根右左呢，可以先压左树再压右树嘛，然后反过来不就是左右根了吗？（反过来用栈来实现，栈一个很大的作用就是实现逆序）
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
    public List<Integer> postorderTraversal(TreeNode root) {
        Stack<TreeNode> stackA = new Stack<>();
        Stack<TreeNode> stackB = new Stack<>();
        if(root == null) return list;
        stackA.push(root);
        while(!stackA.isEmpty()){
            TreeNode top = stackA.pop();
            stackB.push(top);
            if(top.left != null) stackA.push(top.left);
            if(top.right != null) stackA.push(top.right);
        }
        while(!stackB.isEmpty()){
            list.add(stackB.pop().val);
        }
        return list;
    }
}
```
### 解法三：Morris遍历
构建从下到上的连接，一条路能够走遍所有节点；   
当我们返回上层之后，也就是将连线断开的时候，打印下层的单链表。
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
    public List<Integer> postorderTraversal(TreeNode root) {
        if(root == null) return list;
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
                    postMorrisPrint(cur.left); //第二次到达时打印下一层的单链表；
                    cur = cur.right;
                }
            }else{
                cur = cur.right;
            }
        }
        postMorrisPrint(root);
        return list;
    }
    private void postMorrisPrint(TreeNode node){
        TreeNode reverseList = reverseList(node); //反转单链表；
        TreeNode cur = reverseList;
        while(cur != null){
            list.add(cur.val);
            cur = cur.right;
        }
        reverseList(reverseList);
    }
    private TreeNode reverseList(TreeNode node){
        TreeNode pre = null;
        TreeNode cur = node;
        while(cur != null){
            TreeNode next = cur.right;
            cur.right = pre;
            pre = cur;
            cur = next;
        }
        return pre;
    }
}
```
### 体会
后序遍历的特殊在于其Morris遍历；

### 相关链接
[二叉树](https://www.cnblogs.com/Curryxin/p/15063475.html)
