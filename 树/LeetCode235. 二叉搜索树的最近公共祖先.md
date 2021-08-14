## 235. 二叉搜索树的最近公共祖先
> **知识点：二叉树；递归**
### 题目描述

给定一个二叉搜索树, 找到该树中两个指定节点的最近公共祖先。

百度百科中最近公共祖先的定义为：“对于有根树 T 的两个结点 p、q，最近公共祖先表示为一个结点 x，满足 x 是 p、q 的祖先且 x 的深度尽可能大（一个节点也可以是它自己的祖先）。”

例如，给定如下二叉搜索树:  root = [6,2,8,0,4,7,9,null,null,3,5]

##### 示例
![image](https://note.youdao.com/yws/public/resource/6fa0eca998f3cbca5812e4ebbe017e5e/xmlnote/94E7E98AE66D44B5B39CD0853839170F/11472)
```
输入: root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 8
输出: 6 
解释: 节点 2 和节点 8 的最近公共祖先是 6。

输入: root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 4
输出: 2
解释: 节点 2 和节点 4 的最近公共祖先是 2, 因为根据定义最近公共祖先节点可以为节点本身。

```
---
### 解法一：递归
函数功能：寻找两个节点的最近公共祖先  
1.终止条件：root == null，返回null；  
2.该做什么：给了两个节点怎么判断最近公共祖先呢，如果两个节点一个大于root值，一个小于root值，那显然root就是最近公共祖先，或者有个等于root，有个大或小，root也是最近公共祖先，如果都大或者都小，那就分别交给右树或左树去处理就行了。  
3.什么时候做：需要根据两个节点和root的关系去看交给哪个树，所以是前序。

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
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if(root == null) return null;
        if(root.val > p.val && root.val > q.val) return lowestCommonAncestor(root.left,p,q);
        if(root.val < p.val && root.val < q.val) return lowestCommonAncestor(root.right, p, q);
        return root;  //其他情况都返回root；
    }
}
```
空间复杂度：O(N):堆栈的支出
### 解法二：迭代
从根节点开始遍历，找到p和q的路径，找到两个的分叉点就可以了。

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
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        TreeNode ancestor = root;
        while(true){
            if(p.val < ancestor.val && q.val < ancestor.val){  //都往左或右走；
                ancestor = ancestor.left;
            }else if(p.val > ancestor.val && q.val > ancestor.val){
                ancestor = ancestor.right;
            }else{
                break;  //只要出现一个和ancestor相等的或者一个左一个右的，就是了；
            }
        }
        return ancestor;
    }
}
```
