## 129. 求根节点到叶节点数字之和
> **知识点：二叉树；递归**
### 题目描述

给你一个二叉树的根节点 root ，树中每个节点都存放有一个 0 到 9 之间的数字。
每条从根节点到叶节点的路径都代表一个数字：

例如，从根节点到叶节点的路径 1 -> 2 -> 3 表示数字 123 。
计算从根节点到叶节点生成的 所有数字之和 。

叶节点 是指没有子节点的节点。

##### 示例
```
输入：root = [1,2,3]
输出：25
解释：
从根到叶子节点路径 1->2 代表数字 12
从根到叶子节点路径 1->3 代表数字 13
因此，数字总和 = 12 + 13 = 25

输入：root = [4,9,0,5,1]
输出：1026
解释：
从根到叶子节点路径 4->9->5 代表数字 495
从根到叶子节点路径 4->9->1 代表数字 491
从根到叶子节点路径 4->0 代表数字 40
因此，数字总和 = 495 + 491 + 40 = 1026
```
![image](https://note.youdao.com/yws/public/resource/6fa0eca998f3cbca5812e4ebbe017e5e/xmlnote/23B9C719DACF48CDAA7279A5709B1EC8/9907)
---
### 解法一：DFS

函数功能：得到从根节点到叶子节点的数字之和
1.终止条件：root 为空，返回0；    
2.该做什么：节点想要获得自己的值需要能够得到上层的值，自己的值就是上层的值*10+自己。然后看一下自己是不是叶子节点，是的话就加上，不是就接着处理子树；    
3.什么时候做：这很明显是从上往下走：前序；

二叉树的每条从根节点到叶子节点的路径都代表一个数字。其实，每个节点都对应一个数字，等于其父节点对应的数字乘以 10 再加上该节点的值。只要计算出每个叶子节点对应的数字，然后计算所有叶子节点对应的数字之和，即可得到结果。

从根节点开始，遍历每个节点，如果遇到叶子节点，则将叶子节点对应的数字加到数字之和。如果当前节点不是叶子节点，则计算其子节点对应的数字，然后对子节点递归遍历。

三要素：    
1.dfs的功能：计算叶子节点对应的和；   
2.终止条件：节点为空为0；到达叶子节点，就返回；  
3.缩小范围：这是采用的模型一，在递去的过程中解决问题，在递去时依次计算出每个节点对应的值，然后判断此节点是否是叶子节点，是就返回，不是就缩小范围进行递归。

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
    public int sumNumbers(TreeNode root) {
        //因为我们得从上一层得到值；所以传两个参数；
        return sumNumbers(root, 0);
    }
    private int sumNumbers(TreeNode root, int n){
        if(root == null) return 0;
        int temp = n*10 + root.val;
        if(root.left == null && root.right == null){
            return temp;
        }
        return sumNumbers(root.left, temp) + sumNumbers(root.right, temp);
    }
}
```

