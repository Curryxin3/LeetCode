## 102. 二叉树的层序遍历
> **知识点：二叉树；队列**
### 题目描述

给你一个二叉树，请你返回其按 层序遍历 得到的节点值。 （即逐层地，从左到右访问所有节点）。

##### 示例
```
二叉树：[3,9,20,null,null,15,7],
    3
   / \
  9  20
    /  \
   15   7
   
返回其层序遍历结果：

[
  [3],
  [9,20],
  [15,7]
]。
```
---
### 解法一：层序遍历
这就是二叉树的层序遍历  
借用队列结构，依次入队，依次出队。  
每次节点出队同时入队其左右节点。
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
    List<List<Integer>> list;
    public List<List<Integer>> levelOrder(TreeNode root) {
        list = new ArrayList<>();
        if(root == null) return list;
        Queue<TreeNode> queue = new LinkedList<>();
        queue.add(root);
        while(!queue.isEmpty()){
            List<Integer> listin = new ArrayList<>();
            int levelSize = queue.size();
            for(int i = 0; i < levelSize; i++){
                TreeNode front = queue.poll();
                listin.add(front.val);
                if(front.left != null) queue.add(front.left);
                if(front.right != null) queue.add(front.right);
            }
            list.add(listin);
        }
        return list;
    }
}
```

