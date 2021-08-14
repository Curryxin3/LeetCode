## 225. 用队列实现栈
> **知识点：栈；队列；**
### 题目描述

请你仅使用两个队列实现一个后入先出（LIFO）的栈，并支持普通栈的全部四种操作（push、top、pop 和 empty）。

实现 MyStack 类：

- void push(int x) 将元素 x 压入栈顶。
- int pop() 移除并返回栈顶元素。
- int top() 返回栈顶元素。
- boolean empty() 如果栈是空的，返回 true ；否则，返回 false 。

**注意**：
- 你只能使用队列的基本操作 —— 也就是 push to back、peek/pop from front、size 和 is empty 这些操作。
- 你所使用的语言也许不支持队列。 你可以使用 list （列表）或者 deque（双端队列）来模拟一个队列 , 只要是标准的队列操作即可。


##### 示例

```
输入：
["MyStack", "push", "push", "top", "pop", "empty"]
[[], [1], [2], [], [], []]
输出：
[null, null, null, 2, 2, false]

解释：
MyStack myStack = new MyStack();
myStack.push(1);
myStack.push(2);
myStack.top(); // 返回 2
myStack.pop(); // 返回 2
myStack.empty(); // 返回 False
```
---
### 解法一：两个队列
一个队列来实现和栈一样的存储，关键就在于：**队首必须是最后一个入栈的元素**；这样才能实现后入先出。      
另一个队列用来打辅助；
```
class MyStack {

    Queue<Integer> A, B;
    /** Initialize your data structure here. */
    public MyStack() {
        A = new LinkedList();
        B = new LinkedList();
    }
    
    /** Push element x onto stack. */
    public void push(int x) {
        B.add(x);
        while(!A.isEmpty()){
            B.add(A.poll());
        }
        Queue<Integer> temp = A;
        A = B;
        B = temp;
    }
    
    /** Removes the element on top of the stack and returns that element. */
    public int pop() {
        return A.poll();
    }
    
    /** Get the top element. */
    public int top() {
        return A.peek();
    }
    
    /** Returns whether the stack is empty. */
    public boolean empty() {
        return A.isEmpty();
    }
}

/**
 * Your MyStack object will be instantiated and called as such:
 * MyStack obj = new MyStack();
 * obj.push(x);
 * int param_2 = obj.pop();
 * int param_3 = obj.top();
 * boolean param_4 = obj.empty();
 */
```
### 解法二：一个队列
仔细观察上面的过程，我们只要抓住核心：**队列的首元素即为要入栈的元素**；
所以可以将入栈的元素，先进入队尾，然后把队首依次出队再接到队尾的后面；这样之前的队尾就变成了队首；
```
class MyStack {
    Queue<Integer> A;
    /** Initialize your data structure here. */
    public MyStack() {
        A = new LinkedList();
    }
    
    /** Push element x onto stack. */
    public void push(int x) {
        int l = A.size();
        A.add(x);
        for(int i = 0; i < l; i++){
            A.add(A.poll());
        }
    }
    
    /** Removes the element on top of the stack and returns that element. */
    public int pop() {
        return A.poll();
    }
    
    /** Get the top element. */
    public int top() {
        return A.peek();
    }
    
    /** Returns whether the stack is empty. */
    public boolean empty() {
        return A.isEmpty();
    }
}

/**
 * Your MyStack object will be instantiated and called as such:
 * MyStack obj = new MyStack();
 * obj.push(x);
 * int param_2 = obj.pop();
 * int param_3 = obj.top();
 * boolean param_4 = obj.empty();
 */
```
### 参考链接
[用队列实现栈](https://leetcode-cn.com/problems/implement-stack-using-queues/solution/yong-dui-lie-shi-xian-zhan-by-leetcode-solution/)





