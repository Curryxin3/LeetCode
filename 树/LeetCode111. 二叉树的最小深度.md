## 111. 二叉树的最小深度
> **知识点：二叉树，递归**
### 题目描述

给定一个二叉树，找出其最小深度。

最小深度是从根节点到最近叶子节点的最短路径上的节点数量。

说明：叶子节点是指没有子节点的节点。

![image](https://note.youdao.com/yws/public/resource/6fa0eca998f3cbca5812e4ebbe017e5e/xmlnote/E0176E42F1734A78A1DB88395F296683/9972)

##### 示例
```
输入：root = [3,9,20,null,null,15,7]
输出：2

输入：root = [2,null,3,null,4,null,5,null,6]
输出：5
```
---
### 解法一：递归法

函数功能：求一颗二叉树的最小深度      
1.终止条件：root == null， return 0；    
2.该做什么：此题和最大深度的区别是什么，最大深度只要取两颗树里最大的就可以了，但是最小深度不同，比如如果其左子树或右子树为空的时候，只能去递归调用它的孩子，如果两个孩子都有，那就可以取两个里面小的深度+1了。    
3.什么时候做，在调用下面的子树的时候我们需要先判断，所以先序。


此题不能像最大深度那样直接求两颗子树的最大然后+1，最大深度可以是因为取大值不会影响一棵树为空的时候。但是取最小就不一样了，如果一棵树为空，那最小的应该是不为空的那边的值，但是还按原来方式就变成了0+1；所以，在处理每一个节点的时候，如果有两个孩子，那就可以继续取小+1，如果只有一个孩子，那就只能去递归它的孩子。
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
    public int minDepth(TreeNode root) {
        if(root == null) return 0;
        if(root.left == null && root.right == null) return 1;
        if(root.left == null && root.right != null) return minDepth(root.right)+1;
        if(root.right == null && root.left != null) return minDepth(root.left)+1; 
        return Math.min(minDepth(root.left), minDepth(root.right))+1;
    }
}
```
### 解法二：层次遍历（BFS)
利用队列的结构，一层一层的遍历；节点不停的出队入队，每遍历一个节点的时候，将其左节点和右节点入队。注意在遍历的时候判断节点是否有孩子，如果没孩子了，那就可以返回了。
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
    public int minDepth(TreeNode root) {
        Queue<TreeNode> queue = new LinkedList<>();
        if(root == null) return 0;
        queue.add(root);
        int level = 0;
        while(!queue.isEmpty()){
            level++;
            int levelNum = queue.size();
            for(int i = 0; i < levelNum; i++){
                TreeNode front = queue.poll();
                if(front.left == null && front.right == null) return level;
                if(front.left != null) queue.add(front.left);
                if(front.right != null) queue.add(front.right);
            }
        }
        return level;
    }
}
```

### 相关题目
[求二叉树的深度](https://www.cnblogs.com/Curryxin/p/15065335.html)
