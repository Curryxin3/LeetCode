## 133. 克隆图
> **知识点：图；递归**
### 题目描述

给你无向 连通 图中一个节点的引用，请你返回该图的 深拷贝（克隆）。

图中的每个节点都包含它的值 val（int） 和其邻居的列表（list[Node]）。

```
class Node {
    public int val;
    public List<Node> neighbors;
}
```

##### 示例
![image](https://note.youdao.com/yws/public/resource/d7e7b9261eeda158304934942a33df1b/xmlnote/751A0D070B104A1B94DBEE60B8FE1A36/11665)
```
输入：adjList = [[2,4],[1,3],[2,4],[1,3]]
输出：[[2,4],[1,3],[2,4],[1,3]]
解释：
图中有 4 个节点。
节点 1 的值是 1，它有两个邻居：节点 2 和 4 。
节点 2 的值是 2，它有两个邻居：节点 1 和 3 。
节点 3 的值是 3，它有两个邻居：节点 2 和 4 。
节点 4 的值是 4，它有两个邻居：节点 1 和 3 。

输入：adjList = [[]]
输出：[[]]
解释：输入包含一个空列表。该图仅仅只有一个值为 1 的节点，它没有任何邻居。

输入：adjList = []
输出：[]
解释：这个图是空的，它不含任何节点。

输入：adjList = [[2],[1]]
输出：[[2],[1]]
```
---
### 解法一：深度优先(DFS)    

图的深拷贝是在做什么，对于一张图而言，它的深拷贝即构建一张与原图结构，值均一样的图，但是其中的节点不再是原来图节点的引用。   

所以需要进行图的遍历，图的遍历有两种方法：DFS和BFS，为了避免陷入死循环，需要定义一个结构来存储我们已经遍历过的节点，不然从1能到2，到2后又会回到1，所以使用哈希表来存储遍历到的节点，key是遍历到的节点，value是创建的克隆节点，如果遍历过，那直接返回创建的克隆节点。    

函数功能：克隆图，其实就是创建节点，然后填充好neighbors。   
1.终止条件： node==null， 直接返回node；  
2.该做什么：就是创建克隆节点，然后填满邻居节点；所以首先判断有没有在map里，有了的话证明来过了，直接返回克隆的节点就可以，没有的话就创建节点，并且将其放入map中，然后就该填充邻居节点了，递归调用。  
3.什么时候做，先创建节点，然后填充调用，先序。
```
/*
// Definition for a Node.
class Node {
    public int val;
    public List<Node> neighbors;
    public Node() {
        val = 0;
        neighbors = new ArrayList<Node>();
    }
    public Node(int _val) {
        val = _val;
        neighbors = new ArrayList<Node>();
    }
    public Node(int _val, ArrayList<Node> _neighbors) {
        val = _val;
        neighbors = _neighbors;
    }
}
*/

class Solution {
    Map<Node, Node> vis = new HashMap<>();
    public Node cloneGraph(Node node) {
        if(node == null) return null;
        if(vis.containsKey(node)){
            return vis.get(node);  //访问过了就从表里直接取出克隆的节点；
        }
        Node cloneNode = new Node(node.val, new ArrayList());
        //创建之后放到哈希表里；
        vis.put(node, cloneNode);
        //遍历邻居并更新；
        for(Node neighbor : node.neighbors){
            cloneNode.neighbors.add(cloneGraph(neighbor)); //注意邻居节点是克节点；
        }
        return cloneNode;
    }
}
```

### 解法二：广度优先(BFS)  

和深度一样需要有个map来判断是否遍历过了，使用BFS，创建一个队列，然后将各节点依次入队。入队头节点，然后取出，遍历出队的邻居节点，如果没有被访问过，那就入队，克隆并且添加到map中，如果访问过了，那就更新克隆节点的邻居节点就可以了。  

```
/*
// Definition for a Node.
class Node {
    public int val;
    public List<Node> neighbors;
    public Node() {
        val = 0;
        neighbors = new ArrayList<Node>();
    }
    public Node(int _val) {
        val = _val;
        neighbors = new ArrayList<Node>();
    }
    public Node(int _val, ArrayList<Node> _neighbors) {
        val = _val;
        neighbors = _neighbors;
    }
}
*/

class Solution {
    public Node cloneGraph(Node node) {
        Map<Node, Node> vis = new HashMap<>();
        if(node == null) return null;
        Queue<Node> queue = new LinkedList<>();
        queue.add(node); //首节点入队；
        vis.put(node, new Node(node.val, new ArrayList())); //克隆节点并入表；
        while(!queue.isEmpty()){
            Node head = queue.poll();
            for(Node neighbor : head.neighbors){
                if(!vis.containsKey(neighbor)){
                    vis.put(neighbor, new Node(neighbor.val, new ArrayList())); 
                    queue.add(neighbor); //依次设置访问过并入队；
                }
                vis.get(head).neighbors.add(vis.get(neighbor)); //添加邻居，注意是添加的克隆的；
            }
        }
        return vis.get(node);
    }
}
```

### 相关链接  
[克隆图](https://leetcode-cn.com/problems/clone-graph/solution/ke-long-tu-by-leetcode-solution/)
