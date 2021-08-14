## 剑指 Offer 30. 包含min函数的栈
> **知识点：栈；单调**
### 题目描述

定义栈的数据结构，请在该类型中实现一个能够得到栈的最小元素的 min 函数在该栈中，调用 min、push 及 pop 的时间复杂度都是 O(1)。。

##### 示例

```
MinStack minStack = new MinStack();
minStack.push(-2);
minStack.push(0);
minStack.push(-3);
minStack.min();   --> 返回 -3.
minStack.pop();
minStack.top();      --> 返回 0.
minStack.min();   --> 返回 -2.
```
---
### 解法一：单调

我们怎么能在常数时间内获得栈内的最小元素呢，可以采用一个辅助栈，这个辅助栈是维持一个单调递减，这样栈顶就始终是一个最小值。  

细节就是注意弹出元素的时候，检查辅助栈栈顶是否和弹出元素一样，一样的话就弹出，不一样就留着；

```
class MinStack {
     Stack<Integer> stackA;
     Stack<Integer> stackB; //辅助的单调栈；
    /** initialize your data structure here. */
    public MinStack() {
        stackA = new Stack<>();
        stackB = new Stack<>();
    }
    
    public void push(int x) {
        stackA.push(x);
        if(stackB.isEmpty() || x <= stackB.peek()){
            stackB.push(x); //维持一个单调递减栈；
        }
    }
    
    public void pop() {
        int x = stackA.pop();
        if(x == stackB.peek()){
            stackB.pop();  //如果栈顶和弹出的相等，那就弹出；
        }
    }
    
    public int top() {
        return stackA.peek();
    }
    
    public int min() {
        return stackB.peek();
    }
}

/**
 * Your MinStack object will be instantiated and called as such:
 * MinStack obj = new MinStack();
 * obj.push(x);
 * obj.pop();
 * int param_3 = obj.top();
 * int param_4 = obj.min();
 */
```   
