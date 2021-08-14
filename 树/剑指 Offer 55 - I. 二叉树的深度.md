## 剑指 Offer 55 - I. 二叉树的深度
> **知识点：二叉树，递归**
### 题目描述

输入一棵二叉树的根节点，求该树的深度。从根节点到叶节点依次经过的节点（含根、叶节点）形成树的一条路径，最长路径的长度为树的深度。

![image](https://note.youdao.com/yws/public/resource/6fa0eca998f3cbca5812e4ebbe017e5e/xmlnote/2126CEA7922C40C0990442582F28F322/9655)

##### 示例
```
输入：root = [4,2,7,1,3,6,9]
输出：[4,7,2,9,6,3,1]
```
---
### 解法一：递归法
函数功能：一个树的深度     
1.终止条件：节点为空，深度为0；     
2.该做什么：当前节点为根的树深度是左子树和右子树深度大的+1；    
3.什么时候做：得知道子树的深度才能知道当前树：后序；
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
    public int maxDepth(TreeNode root) {
        if(root == null) return 0;
        return Math.max(maxDepth(root.left), maxDepth(root.right))+1;
    }
}
```
### 解法二：层次遍历（BFS)
利用队列的结构，一层一层的遍历；节点不停的出队入队，每遍历一个节点的时候，将其左节点和右节点入队。
**关键点**：每遍历一层，则计数器加+1；直到遍历完成，得到树的深度。
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
    public int maxDepth(TreeNode root) {
        Queue<TreeNode> queue = new LinkedList<>();
        int count = 0;
        if(root == null) return 0;
        queue.add(root);
        while(!queue.isEmpty()){
            count++;
            int levelNum = queue.size();
            for(int i = 0; i < levelNum; i++){
                TreeNode front = queue.poll();
                if(front.left != null) queue.add(front.left);
                if(front.right != null) queue.add(front.right);
            }
        }
        return count;
    }
}
```

### 相关链接
[二叉树的深度](https://leetcode-cn.com/problems/er-cha-shu-de-shen-du-lcof/solution/mian-shi-ti-55-i-er-cha-shu-de-shen-du-xian-xu-bia/)
