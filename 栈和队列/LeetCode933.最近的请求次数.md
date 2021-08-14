## 933.最近的请求次数
> **知识点：队列；**
### 题目描述

写一个 RecentCounter 类来计算特定时间范围内最近的请求。  
请你实现 RecentCounter 类：  
RecentCounter() 初始化计数器，请求数为 0 。
int ping(int t) 在时间 t 添加一个新请求，其中 t 表示以毫秒为单位的某个时间，并返回过去 3000 毫秒内发生的所有请求数（包括新请求）。确切地说，返回在 [t-3000, t] 内发生的请求数。
保证 每次对 ping 的调用都使用比之前更大的 t 值。
##### 示例

```
输入：
["RecentCounter", "ping", "ping", "ping", "ping"]
[[], [1], [100], [3001], [3002]]
输出：
[null, 1, 2, 3, 3]
解释：
RecentCounter recentCounter = new RecentCounter();
recentCounter.ping(1);     // requests = [1]，范围是 [-2999,1]，返回 1
recentCounter.ping(100);   // requests = [1, 100]，范围是 [-2900,100]，返回 2
recentCounter.ping(3001);  // requests = [1, 100, 3001]，范围是 [1,3001]，返回 3
recentCounter.ping(3002);  // requests = [1, 100, 3001, 3002]，范围是 [2,3002]，返回 3
```
### 解法一
这道题对题目的理解可能就需要费点时间，这啥意思啊，输入输出也看不懂。其实就是说比如我们打电话，看我们在3000秒之间打了几次，其次那个t就是我们每次拨号的时间。这道题典型的是用队列去解的，每次拨号，就把时间t入队，然后每次将入队的去和要出队的，也就是最前面的元素比，看是否大于3000，大于3000，就出队，最后返回队列的个数，也就是3000内的ping的次数。
```
class RecentCounter {
    Queue<Integer> queue;
    public RecentCounter() {
        queue = new LinkedList<>();
    }
    
    public int ping(int t) {
        queue.add(t);
        while(t - queue.peek() > 3000) queue.poll();
        return queue.size();
    }
}

/**
 * Your RecentCounter object will be instantiated and called as such:
 * RecentCounter obj = new RecentCounter();
 * int param_1 = obj.ping(t);
 */
 ```
 
 ### 体会
 熟练掌握队列的基本方法；




