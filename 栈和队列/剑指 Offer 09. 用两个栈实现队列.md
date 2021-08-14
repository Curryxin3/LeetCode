## 剑指 Offer 09. 用两个栈实现队列
> **知识点：栈；队列；**
### 题目描述
用两个栈实现一个队列。队列的声明如下，请实现它的两个函数 appendTail 和 deleteHead ，分别完成在队列尾部插入整数和在队列头部删除整数的功能。(若队列中没有元素，deleteHead 操作返回 -1 )。

##### 示例
```
输入：
["CQueue","appendTail","deleteHead","deleteHead"]
[[],[3],[],[]]
输出：[null,null,3,-1]

输入：
["CQueue","deleteHead","appendTail","appendTail","deleteHead","deleteHead"]
[[],[],[5],[2],[],[]]
输出：[null,-1,null,null,5,2]

```
---
### 解法一：栈
这道题不是解决问题而是实现类的问题，首先要把题目看懂，输入的是个集合，分别调用上面的方法；构造器，入队，出队。栈的特点是先入后出，而队列的特点是先入先出。栈底元素是先入栈的，如果想让它出去，那得先把上面的元素移走，让栈底元素成为栈顶元素；这就需要另一个栈来实现。       
**双栈可以实现列表倒序** ：如果将栈A的元素循环出栈并压到栈B中，那么对于栈A来讲，栈B正好是其倒序；有含三个元素的栈 A = [1,2,3] 和空栈 B = []。若循环执行 A 元素出栈并添加入栈 B ，直到栈 A 为空，则 A = [] , B = [3,2,1] ，即 栈 B 元素实现栈 A 元素倒序 。        
所以我们可以用栈B来实现删除的功能，B执行出栈后就就相当于删除了A的栈底元素，即队首元素。

```
class CQueue {
    Stack<Integer> A,B;
    public CQueue() {
        A = new Stack<>();
        B = new Stack<>();
    }
    
    public void appendTail(int value) {
        A.push(value);
    }
    
    public int deleteHead() {
        if(!B.isEmpty()) return B.pop();
        if(A.isEmpty()) return -1;
        while(!A.isEmpty()){
            B.push(A.pop());
        }
        return B.pop();
    }
}

/**
 * Your CQueue object will be instantiated and called as such:
 * CQueue obj = new CQueue();
 * obj.appendTail(value);
 * int param_2 = obj.deleteHead();
 */
```

### 体会
栈和队列的特点；熟悉常用方法。
### 参考链接
[用两个栈实现队列](https://leetcode-cn.com/problems/yong-liang-ge-zhan-shi-xian-dui-lie-lcof/solution/mian-shi-ti-09-yong-liang-ge-zhan-shi-xian-dui-l-2/)
