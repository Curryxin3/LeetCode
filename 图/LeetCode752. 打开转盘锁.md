## 752. 打开转盘锁
> **知识点：图；BFS**
### 题目描述

你有一个带有四个圆形拨轮的转盘锁。每个拨轮都有10个数字： '0', '1', '2', '3', '4', '5', '6', '7', '8', '9' 。每个拨轮可以自由旋转：例如把 '9' 变为 '0'，'0' 变为 '9' 。每次旋转都只能旋转一个拨轮的一位数字。

锁的初始数字为 '0000' ，一个代表四个拨轮的数字的字符串。

列表 deadends 包含了一组死亡数字，一旦拨轮的数字和列表里的任何一个元素相同，这个锁将会被永久锁定，无法再被旋转。

字符串 target 代表可以解锁的数字，你需要给出解锁需要的最小旋转次数，如果无论如何不能解锁，返回 -1 。

##### 示例

```
输入：deadends = ["0201","0101","0102","1212","2002"], target = "0202"
输出：6
解释：
可能的移动序列为 "0000" -> "1000" -> "1100" -> "1200" -> "1201" -> "1202" -> "0202"。
注意 "0000" -> "0001" -> "0002" -> "0102" -> "0202" 这样的序列是不能解锁的，
因为当拨动到 "0102" 时这个锁就会被锁定。

输入: deadends = ["8888"], target = "0009"
输出：1
解释：
把最后一位反向旋转一次即可 "0000" -> "0009"。

输入: deadends = ["8887","8889","8878","8898","8788","8988","7888","9888"], target = "8888"
输出：-1
解释：
无法旋转到目标数字且不被锁定。

输入: deadends = ["0000"], target = "8888"
输出：-1
```
---
### 解法一：BFS

这道题目看起来很绕，其实透过这个场景去看本质，其实就是一幅图，什么意思呢，每个密码就可以看做是图的节点，目标就是找到target的节点，怎么办，遍历呗，啥时候找到啥时候停，而图的遍历自然要用到BFS，每个节点其实都有8个相邻节点，因为它转动后下一个可能的密码就是8个，举个例子，比如"0000"转一下以后"1000,9000,0100,0900,..."，这就是相邻节点；然后还有用一个visited来记录已经遍历过的，防止回头路，此外如果有密码等于deadend了，那就跳过不用遍历这个密码了。 


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
题解：
```
class Solution {
    public int openLock(String[] deadends, String target) {
        Set<String> dead = new HashSet<>();
        for(String s:deadends){
            dead.add(s);
        }
        Queue<String> queue = new LinkedList<>();
        queue.offer("0000");
        Set<String> visited = new HashSet<>();
        visited.add("0000");  //记录所有已经遍历到的密码；
        int step = 0;
        while(!queue.isEmpty()){
            int size = queue.size();
            for(int i = 0; i < size; i++){
                String cur = queue.poll();
                //截止条件；
                if(dead.contains(cur)) continue;
                if(cur.equals(target)) return step;
                //处理相邻节点；一共8个；
                for(int j = 0; j < 4; j++){
                    String up = plusOne(cur, j);
                    if(!visited.contains(up)){
                        visited.add(up);
                        queue.offer(up);
                    }
                    String down = minusOne(cur, j);
                    if(!visited.contains(down)){
                        visited.add(down);
                        queue.offer(down);
                    }
                }
            }
            step++;
        }
        return -1;
    }
    private String plusOne(String s, int j){
        char[] ch = s.toCharArray();
        if(ch[j] == '9') ch[j] = '0';
        else ch[j] += 1;
        return new String(ch);
    }
    private String minusOne(String s, int j){
        char[] ch = s.toCharArray();
        if(ch[j] == '0') ch[j] = '9';
        else ch[j] -= 1;
        return new String(ch);
    }
}
```
仔细看一下上述题解，基本上是和框架也就是模板是一样的，要注意灵活变通。
