## 155. 最小栈
> **知识点：栈；单调**
### 题目描述

设计一个支持 push ，pop ，top 操作，并能在常数时间内检索到最小元素的栈。

push(x) —— 将元素 x 推入栈中。
pop() —— 删除栈顶的元素。
top() —— 获取栈顶元素。
getMin() —— 检索栈中的最小元素。

##### 示例

```
输入：
["MinStack","push","push","push","getMin","pop","top","getMin"]
[[],[-2],[0],[-3],[],[],[],[]]

输出：
[null,null,null,null,-3,null,0,-2]

解释：
MinStack minStack = new MinStack();
minStack.push(-2);
minStack.push(0);
minStack.push(-3);
minStack.getMin();   --> 返回 -3.
minStack.pop();
minStack.top();      --> 返回 0.
minStack.getMin();   --> 返回 -2.

```
---
### 解法一：单调

经典的用空间换取时间的问题，可以使用一个辅助栈，用来存储当前堆中的最小值，如果新堆的元素比辅助栈栈顶要小，那就可以更新为新的元素，如果比栈顶要大，那就再存一次栈顶。

```
class MinStack {
    Stack<Integer> stackA;
    Stack<Integer> stackB;
    /** initialize your data structure here. */
    public MinStack() {
        stackA = new Stack<>();
        stackB = new Stack<>();
    }
    
    public void push(int val) {
        stackA.push(val);
        if(stackB.isEmpty() || val <= stackB.peek()){
            stackB.push(val);
        }
    }
    
    public void pop() {
        if(!stackA.isEmpty()){
            int top = stackA.pop();  //这是一个细节，注意不能直接拿stackA.pop() == stackB.peek(),
            //因为两个是包装类，所以==比较的是内存地址，所以可以这样写，会自动装箱；
            if(top == stackB.peek()){
                stackB.pop();
            }
        }
    }
    
    public int top() {
        if(!stackA.isEmpty()){
            return stackA.peek();
        }
        return 0;
    }
    
    public int getMin() {
        if(!stackB.isEmpty()){
            return stackB.peek();
        }
        return 0;
    }
}

/**
 * Your MinStack object will be instantiated and called as such:
 * MinStack obj = new MinStack();
 * obj.push(val);
 * obj.pop();
 * int param_3 = obj.top();
 * int param_4 = obj.getMin();
 */
```
