## 剑指 Offer 59 - II. 队列的最大值
> **知识点：队列；单调**
### 题目描述

请定义一个队列并实现函数 max_value 得到队列里的最大值，要求函数max_value、push_back 和 pop_front 的均摊时间复杂度都是O(1)。

若队列为空，pop_front 和 max_value 需要返回 -1

##### 示例

```
输入: 
["MaxQueue","push_back","push_back","max_value","pop_front","max_value"]
[[],[1],[2],[],[],[]]
输出: [null,null,null,2,1,2]

输入: 
["MaxQueue","pop_front","max_value"]
[[],[],[]]
输出: [null,-1,-1]
```
---
### 解法一：双端队列+单调

**当一个元素进入队列时，它前面所有比它小的元素就不会再对答案产生影响了**   

比如数字序列 1 1 1 1 2，那么在第一个数字 2 被插入后，数字 2 前面的所有数字 1 将不会对结果产生影响。因为如果数字 1 在队列中，那么数字 2 必然也在队列中(先进先出)，使得数字 1 对结果没有影响。

所以当从队列尾部插入元素时，可以把没有影响的，也就是所有比要插入元素小的数字都取出来，使得队列中只保留对结果有影响的数字，也就是要求队列单调递减；  

在元素入队时，判断当前元素和队尾的关系，把所有小于当前值的队里元素都移除；  

所以可以在添加和删除的时候采用普通的队列；     
在取出最大值的时候采用双端队列；**双端队列维持递减**，所以每次取出队首元素即可；

```
class MaxQueue {
    Queue<Integer> queue;
    Deque<Integer> deque;
    public MaxQueue() {
        queue = new LinkedList<>();
        deque = new LinkedList<>();
    }
    
    public int max_value() {
        if(deque.isEmpty()) return -1;
        return deque.peekFirst();
    }
    
    public void push_back(int value) {
        queue.offer(value);
        while(!deque.isEmpty() && value > deque.peekLast()){
            deque.removeLast();  //维持一个单调递减队列；
        }
        deque.offerLast(value);
    }
    
    public int pop_front() {
        if(queue.isEmpty()) return -1;
        int ans = queue.poll();
        if(ans == deque.peekFirst()){
            deque.removeFirst();
        }
        return ans;
    }
}

/**
 * Your MaxQueue object will be instantiated and called as such:
 * MaxQueue obj = new MaxQueue();
 * int param_1 = obj.max_value();
 * obj.push_back(value);
 * int param_3 = obj.pop_front();
 */
```
