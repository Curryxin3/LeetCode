## 895. 最大频率栈
> **知识点：栈；哈希表**
### 题目描述

实现 FreqStack，模拟类似栈的数据结构的操作的一个类。

FreqStack 有两个函数：

push(int x)，将整数 x 推入栈中。
pop()，它移除并返回栈中出现最频繁的元素。
如果最频繁的元素不只一个，则移除并返回最接近栈顶的元素。

##### 示例

```
输入：
["FreqStack","push","push","push","push","push","push","pop","pop","pop","pop"],
[[],[5],[7],[5],[7],[4],[5],[],[],[],[]]
输出：[null,null,null,null,null,null,null,5,7,5,4]
解释：
执行六次 .push 操作后，栈自底向上为 [5,7,5,7,4,5]。然后：

pop() -> 返回 5，因为 5 是出现频率最高的。
栈变成 [5,7,5,7,4]。

pop() -> 返回 7，因为 5 和 7 都是频率最高的，但 7 最接近栈顶。
栈变成 [5,7,5,4]。

pop() -> 返回 5 。
栈变成 [5,7,4]。

pop() -> 返回 4 。
栈变成 [5,7]。
```
---
### 解法一：哈希表
这个题目需要关注两个点：
- 频率表：对于每个元素，我们要统计出其出现的频率，这自然的就会想到哈希表；用一个频率哈希表存储val和对应的freq。
- 对于频率相同的，要按照离顶近的先出，也就是说后入的先出，所以可以再来一个哈希表，key值是频率，value是一个栈，栈里存放着元素，所以每次弹出的时候都是弹出最大频率的栈顶。
- 注意一个点，就是这个maxFreq一定是连续的，因为在入栈的时候，对于在最大频率处的元素，其也一定存在于比它小的栈里，都是逐步增大才到最大频率栈里的。
```
class FreqStack {
        int maxFreq = 0;
        HashMap<Integer, Integer> valFreq;
        HashMap<Integer, Stack<Integer>> sameFreqVal;
    public FreqStack() {
        valFreq = new HashMap<>();
        sameFreqVal = new HashMap<>();
    }
    
    public void push(int val) {
        int freq = valFreq.getOrDefault(val, 0)+1;  //更新频率表；
        valFreq.put(val, freq);

        sameFreqVal.putIfAbsent(freq, new Stack<Integer>());  //if有key，就不添加，没有key的话就添加；
        sameFreqVal.get(freq).push(val);

        maxFreq = Math.max(freq, maxFreq);
    }
    
    public int pop() {
        Stack<Integer> temp = sameFreqVal.get(maxFreq);  //最大频率对应的元素；

        int top = temp.pop();
        if(temp.isEmpty()){   //弹出后当前栈为空，那就把这条记录删除；
            sameFreqVal.remove(maxFreq);
            maxFreq--;   //频率一定是连续的，因为if一个数在最大频率栈，那在比它频率小的栈里也一定有；
        }
        int freq = valFreq.get(top);
        if(freq-- == 0){
            valFreq.remove(top);
        }else{
            valFreq.put(top, freq--);  //更新频率表；
        }
        return top;
    }
}

/**
 * Your FreqStack object will be instantiated and called as such:
 * FreqStack obj = new FreqStack();
 * obj.push(val);
 * int param_2 = obj.pop();
 */
```
