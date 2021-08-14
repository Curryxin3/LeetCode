## 1162. 地图分析
> **知识点：图；递归**
### 题目描述

你现在手里有一份大小为 N x N 的 网格 grid，上面的每个 单元格 都用 0 和 1 标记好了。其中 0 代表海洋，1 代表陆地，请你找出一个海洋单元格，这个海洋单元格到离它最近的陆地单元格的距离是最大的。

我们这里说的距离是「曼哈顿距离」（ Manhattan Distance）：(x0, y0) 和 (x1, y1) 这两个单元格之间的距离是 |x0 - x1| + |y0 - y1| 。

如果网格上只有陆地或者海洋，请返回 -1。

##### 示例
![image](https://note.youdao.com/yws/public/resource/d7e7b9261eeda158304934942a33df1b/xmlnote/7188484B4F6842C3ACD5F8D47E11493F/11723)
```
输入：[[1,0,1],[0,0,0],[1,0,1]]
输出：2
解释： 
海洋单元格 (1, 1) 和所有陆地单元格之间的距离都达到最大
最大距离为 2。
```
![image](https://note.youdao.com/yws/public/resource/d7e7b9261eeda158304934942a33df1b/xmlnote/6A51FF0D8BD04480A7DE23480EDFE527/11726)
```
输入：[[1,0,0],[0,0,0],[0,0,0]]
输出：4
解释： 
海洋单元格 (2, 2) 和所有陆地单元格之间的距离都达到最大，最大距离为 4。
```
---
### 解法一：广度优先(BFS)    

**树的BFS：先把root节点入队，然后再一层一层的遍历。  
图的BFS也是一样的，与树的BFS的区别是：  
1.树只有一个root，而图可以有多个源点，所有首先需要将多个源点入队。  
2.树是有向的因此不需要标志是否访问过，而对于无向图而言，必须得标志是否访问过！并且为了防止某个节点多次入队，需要在入队前将其设置为已访问**！   

树的广度优先搜索：从root开始往子节点上；   
一个源点的广度优先和多个源点的广度优先，参考下面链接 [广度优先搜索](https://leetcode-cn.com/problems/as-far-from-land-as-possible/solution/zhen-liang-yan-sou-huan-neng-duo-yuan-kan-wan-miao/) 

这道题目就是多源广度优先搜索的样题，我们寻找离陆地最远的海洋单元格，可以将其转化为从所有的陆地开始一轮一轮的往外扩，最后的就是最远的。    
第一轮变化以各个陆地为起点，走一格就能到达的海域；第二轮变化是在第一轮的基础上走两格能到达的海域，这样子不断变化，越到后面没有被覆盖的海域离陆地的距离最远，也越接近我们想找到的那个海域，直到地图被全覆盖。

```
class Solution {
    public int maxDistance(int[][] grid) {
        //把所有的陆地先入队；
        int[] dx = {0, 0, 1, -1};
        int[] dy = {1, -1, 0, 0}; //设置四个变化方向；
        Queue<int[]> queue = new LinkedList<>();
        int m = grid.length, n = grid[0].length;
        for(int i = 0; i < m; i++){
            for(int j = 0; j < n; j++){
                if(grid[i][j] == 1){
                    queue.add(new int[]{i, j});
                }
            }
        }
        if(queue.size() == 0 || queue.size() == m*n) return -1; //没有陆地或海洋；
        //从各个陆地一圈一圈向外扩，最后就是最远的；
        int[] point = null; //出队点；
        while(!queue.isEmpty()){
            point = queue.poll();
            //求出此坐标的四个相邻坐标；
            int x = point[0], y = point[1];
            for(int i = 0; i < 4; i++){
                int newX = x + dx[i];
                int newY = y + dy[i];
                if(newX < 0 || newX >= m || newY < 0 || newY >= n || grid[newX][newY] != 0){
                    continue; //剔除无效坐标和周围陆地；
                }
                grid[newX][newY] = grid[x][y]+1; //向外扩；
                queue.add(new int[]{newX, newY});
            }
        }
        return grid[point[0]][point[1]]-1; //返回最后一次出队的坐标点；
    }
}
```
### 体会
树的BFS：先把root节点入队，然后再一层一层的遍历。  
图的BFS也是一样的，与树的BFS的区别是：  
1.树只有一个root，而图可以有多个源点，所有首先需要将多个源点入队。  
2.树是有向的因此不需要标志是否访问过，而对于无向图而言，必须得标志是否访问过！并且为了防止某个节点多次入队，需要在入队前将其设置为已访问！  

### 相关链接  
[地图分析](https://leetcode-cn.com/problems/as-far-from-land-as-possible/solution/jian-dan-java-miao-dong-tu-de-bfs-by-sweetiee/)
