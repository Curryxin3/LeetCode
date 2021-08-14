## 841. 钥匙和房间
> **知识点：图；递归**
### 题目描述

有 N 个房间，开始时你位于 0 号房间。每个房间有不同的号码：0，1，2，...，N-1，并且房间里可能有一些钥匙能使你进入下一个房间。

在形式上，对于每个房间 i 都有一个钥匙列表 rooms[i]，每个钥匙 rooms[i][j] 由 [0,1，...，N-1] 中的一个整数表示，其中 N = rooms.length。 钥匙 rooms[i][j] = v 可以打开编号为 v 的房间。

最初，除 0 号房间外的其余所有房间都被锁住。

你可以自由地在房间之间来回走动。

如果能进入每个房间返回 true，否则返回 false。

##### 示例

```
输入: [[1],[2],[3],[]]
输出: true
解释:  
我们从 0 号房间开始，拿到钥匙 1。
之后我们去 1 号房间，拿到钥匙 2。
然后我们去 2 号房间，拿到钥匙 3。
最后我们去了 3 号房间。
由于我们能够进入每个房间，我们返回 true。

输入：[[1,3],[3,0,1],[2],[0]]
输出：false
解释：我们不能进入 2 号房间。
```
---
### 解法一：深度优先(DFS)    

当 x 号房间中有 y 号房间的钥匙时，我们就可以从 x 号房间去往 y 号房间。如果我们将这 n 个房间看成有向图中的 n 个节点，那么上述关系就可以看作是图中的 x 号点到 y 号点的一条有向边。

这样一来，问题就变成了给定一张有向图，询问从 0 号节点出发是否能够到达所有的节点。

深度优先的意思就是先进入第一个房，然后拿出第一把钥匙，去开下一个房，然后再拿到这个房的第一个钥匙，再去开下一个，直到没钥匙了，退回上一个房，重复以往。这是一个递归的过程，和二叉树的遍历一样。  

可以用一个数组来记录到达过哪个房间，防止重复计算。    
函数功能：遍历房间   
1.终止条件：把第0个房间的钥匙拿完；  
2.该做什么：每个房间应该做什么呢，判断数组标志那个房间去过没有，去过了就跳过，没去过就进去，并把标志位置为true。计数器加1.最后看是否等于房间个数。
```
class Solution {
    boolean[] vis; 
    int num = 0;
    public boolean canVisitAllRooms(List<List<Integer>> rooms) {
        int n = rooms.size();
        vis = new boolean[n]; //设置第n个房间是否打开过的标志位；
        canVisitAllRooms(rooms, 0);
        return num == n; //查看是否遍历完；
    }
    private void canVisitAllRooms(List<List<Integer>> rooms, int roomnum){
        vis[roomnum] = true;
        num++;
        for(int i : rooms.get(roomnum)){
            if(!vis[i]){ //没有访问过就进去；
                canVisitAllRooms(rooms, i);
            }
        }
    }
}
```

### 解法二：广度优先(BFS)  

广度优先的意思就是我们不拿到一个钥匙走到头了，我们一个房间一个房间的进，拿到第一个房间的钥匙，把第一个房间里的钥匙对应的门都打开，然后再去第二个房间。这样一个一个的进。   

```
class Solution {
    public boolean canVisitAllRooms(List<List<Integer>> rooms) {
        int n = rooms.size();
        boolean[] vis = new boolean[n]; //标志位就是防止被重复遍历；
        int num = 0;
        Queue<Integer> queue = new LinkedList<>();
        vis[0] = true; //设置0号访问过，为了防止节点多次入队，需要在入队前将其设置为已访问；
        queue.add(0); //0号房间入队；
        while(!queue.isEmpty()){
            int x = queue.poll();
            num++;
            for(int i : rooms.get(x)){
                if(!vis[i]){ //没有被访问过；
                    vis[i] = true;  
                    queue.add(i); //保证入队的都是没有被遍历过的；
                }
            }
        }
        return n == num;
    }
}
```
### 体会

- 1.只要是广度优先的，肯定要用队列，天然是在一起的。  一边弹出，一边遍历，弹出一个以后，就把它的孩子或者是它的关联入队，保证把这一层，或这个节点的都弹出了，接着继续弹下一层或者下一个节点的。  
- 2.树的BFS：先把root节点入队，然后再一层一层的遍历。  
图的BFS也是一样的，与树的BFS的区别是：  
   - 1.树只有一个root，而图可以有多个源点，所有首先需要将多个源点入队。  
   - 2.树是有向的因此不需要标志是否访问过，而对于无向图而言，必须得标志是否访问过！并且为了防止某个节点多次入队，需要在入队前将其设置为已访问！ 
- 3.树用DFS比较多，图用BFS比较多；    

### 相关题目

[133. 克隆图](https://www.cnblogs.com/Curryxin/p/15092925.html)   
[1162. 地图分析](https://www.cnblogs.com/Curryxin/p/15093107.html)
