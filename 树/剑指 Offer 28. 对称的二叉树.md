## 剑指 Offer 28. 对称的二叉树
> **知识点：二叉树，递归**
### 题目描述

请实现一个函数，用来判断一棵二叉树是不是对称的。如果一棵二叉树和它的镜像一样，那么它是对称的。

![image](https://note.youdao.com/yws/public/resource/6fa0eca998f3cbca5812e4ebbe017e5e/xmlnote/E150AA7901DE4EFAAA214616E4F71891/9299)

##### 示例
```
输入：root = [1,2,2,3,4,4,3]
输出：true

输入：root = [1,2,2,null,3,null,3]
输出：false
```
---
### 解法一：递归法

函数功能：判断树是否对称    
1.终止条件：left和right都为null，return true；   
2.最小单元节点做什么：判断是否对称呗：如果有一方到null另一方没到 return false； 如果left和right值不等，return false，判断left的left和right的right是否平衡，判断left的right和right的left是否平衡； 
3.什么时候做：开始遍历的时候就可以去判断了，放在先序的位置上。

我们如何定义两棵子树 a 和 b 是否 “对称” ？

当且仅当两棵子树符合如下要求时，满足 “对称” 要求：

- 1.两棵子树根节点值相同；
- 2.两颗子树的左右子树分别对称，包括：
    - a 树的左子树与 b 树的右子树相应位置的值相等
    - a 树的右子树与 b 树的左子树相应位置的值相等

递归终止条件：
- 1.两边都走到树头了，而且L.val = R.val = null， return true；
- 2.有一边走到头了，即有一个节点为空，L.val == null || R.val == null, return false;
- 3.两个子树的值不相等,下面就不用比了，直接false；L.val != R.val, return false;   

其他情况，说明在这一层上已经相等了，我们则要分别检查 L 和 R 的左右节点是否 “对称” ，即递归调用 check(L.left, R.right) 和 check(L.right, R.left)

递归还是抓住两点本质：   
- 递归的终止条件：上面说的那3条；
- 函数的功能是干什么的，缩小范围：函数就是用来比较两个树是否对称的，往里传树就行了，我能得到结果；从上往下，想要知道整个树是否对称需要知道左右子树是否对称，这是去的过程；知道了左右子树是否对称那就能知道大树是否对称了，这是回的过程。
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
    public boolean isSymmetric(TreeNode root) {
        if(root == null) return true;
        else{
             return isSymmetric(root.left, root.right);
        }
    }
    private boolean isSymmetric(TreeNode left, TreeNode right){
        if( left == null && right == null) return true;
        if( left == null || right == null) return false;
        if( left.val != right.val) return false;
        return isSymmetric(left.left, right.right) && isSymmetric(left.right, right.left);
    }
}
```
时间复杂度；0(N),每个节点恰好被遍历一次；   
空间复杂度；O(N),递归过程中栈的开销；

### 相关链接
[对称二叉树](https://leetcode-cn.com/problems/dui-cheng-de-er-cha-shu-lcof/solution/mian-shi-ti-28-dui-cheng-de-er-cha-shu-di-gui-qing/)
