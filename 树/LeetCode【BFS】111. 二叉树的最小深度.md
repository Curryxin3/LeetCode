## 【BFS】111. 二叉树的最小深度
> **知识点：二叉树，递归；BFS**
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

### 体会

这是一道最经典的使用二叉树的层次遍历的题目，可以按照这个题目总结一下BFS,即广度优先遍历，也叫层次遍历   

先说结论，其实在一棵**二叉树中，用深度优先遍历(DFS)一般要比广度优先遍历(BFS)要多**，每棵树只有两个节点，并且有明显的路径，写起来也简单。但是在**图的相关应用中，用到的更多的是BFS**。所以要很有必要掌握BFS。

BFS解决的问题就是在一幅图(当然可以把树理解为特殊的图)中找到从起点start到target的**最短路径**，这是最典型的应用场景。    

只要用到BFS，就一定会依赖**队列**这个数据结构，其基本思想就是从起点开始入队，队列里存储的是当前树中一层的节点，对应图中就是和当前队列节点所有相邻的节点，然后将当前队列里的元素依次出队，出队的同时就把下一层的节点入队。BFS就是这样子一层一层的遍历；  

- 特点：BFS通俗的来说其实就是“齐头并进”，所有人一起往前冲，**是一种面的结构**；而DFS更像是一种单打独斗，每次一个一个的往前冲，一个走到头了不行了再换另一个，**是一种线的结构**。   
- 缺点：正如上文所说，BFS其实每次都要遍历完树或图中所有节点。其空间复杂度较高。     

BFS算法框架：   
```
//计算起点start到终点target的最小距离
int BDS(Node start, Node target){ 
    Queue<Node> queue;  //核心结构：队列；
    Set<Node> visited;  //在图中都会用到，因为存在着交叉，会走回头路，树中则不需要，因为有next指针；
    queue.offer(start);  //起点入队；
    visited.add(start);  
    int step = 0; //记录扩散步数；
    while(queue.isEmpty()){
        int size = queue.size();
        //从当前队列中所有节点向与其关联的节点扩散；
        for(int i = 0; i < size; i++){
            Node cur = queue.poll();
            if(cur == target){
                return step;  //注意不同题目里这里的判断条件，是否到达终点；
            }
            for(Node x : cur.adj()){  //这里指的是当前节点的邻居节点；
                if(!visited.contains(x)){ //还没走过再加入；不走回头路；
                    queue.offer(x);
                    visited.add(x);
                }
            }
        }
        step++;  //注意在这里更新步数；
    }
}
```

上面就是整体BFS的基本框架，注意要灵活变通，比如说当前节点的相邻节点，对于树肯定就是其孩子，对于二维数组，比如其上下左右都四个位置。

### 相关题目

[752. 打开转盘锁](https://www.cnblogs.com/Curryxin/p/15155184.html)  注意框架的应用   

[求二叉树的深度](https://www.cnblogs.com/Curryxin/p/15065335.html)
