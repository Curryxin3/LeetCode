## 【单调】739. 每日温度
> **知识点：栈；单调**
### 题目描述

请根据每日 气温 列表 temperatures ，请计算在每一天需要等几天才会有更高的温度。如果气温在这之后都不会升高，请在该位置用 0 来代替。

##### 示例

```
输入: temperatures = [73,74,75,71,69,72,76,73]
输出: [1,1,4,2,1,1,0,0]

输入: temperatures = [30,40,50,60]
输出: [1,1,1,0]

输入: temperatures = [30,60,90]
输出: [1,1,0]

```
---
### 解法一：单调

这个题目是很典型的单调栈的题，我们可以维持一个单调递减的栈，也就是栈底是最大的，这样，当来一个新元素的时候判断当前元素和栈顶的大小关系，如果当前元素<栈顶，满足递减栈，可以入栈了；如果当前元素>栈顶，那就证明找到了栈顶对应的下一个更大的，然后再看新的栈顶看还大不大了，如果大了那证明也还是新的栈顶的对应的温度。   

注意我们在这个单调栈里存的是下标，因为最后要返回的是下标，但是这个下标入栈是根据其对应的值来进入的。    

**单调栈在解决这种更大，更小类问题时很常用**。

```
class Solution {
    public int[] dailyTemperatures(int[] temperatures) {
        int index = 0;
        int[] res = new int[temperatures.length];
        Stack<Integer> stack = new Stack<>(); //维持一个单调递减的栈；
        while(index < temperatures.length){
            while(!stack.isEmpty() && temperatures[index] > temperatures[stack.peek()]){
                res[stack.peek()] = index-stack.peek();  //存的是下标；
                stack.pop();
            }
            stack.push(index);
            index++;
        }
        return res;
    }
}
```  

### 体会

- 单调栈、单调队列在解决“**更大、更小**”类问题的时候很常用，其基本思想就是维持一个单调栈或者单调队列，这样在队列首或者是栈顶就是当前已经遍历过的里最小的或者最大的。比如如果想获得序列元素的下一个比其大的，那就可以维持一个单调递减栈，如果新来的元素比栈顶大，那就是栈顶的下一个最大值了，这样可以依次来判断； 
- 这里说的单调递减指的都是从栈底到栈顶；大根堆小根堆说的是堆顶最大还是最小；
- **单调栈的模板(循环里先写处理要弹出的情况！)**
```
//维持一个单调递减栈；
int index = 0;
while(index < nums.length){
    while(!stack.isEmpty() && nums[index] > stack.peek()){
        stack.pop();  //先处理弹出的；
        ...
    }
    stack.push(nums[index]);
    ...
    index++;
}
```
- 单调栈或者队列里可以放数组的值，也可以放下标索引，要灵活取舍；   
- 当需要将单调的容器固定一个容量的时候，很多时候是要用到堆，创建一个容量为k的大根堆或者小根堆；

### 相关题目(单调)  

[155. 最小栈](https://www.cnblogs.com/Curryxin/p/15135700.html)   

[剑指 Offer 30. 包含min函数的栈](https://www.cnblogs.com/Curryxin/p/15130555.html)

[剑指 Offer 59 - I. 滑动窗口的最大值](https://www.cnblogs.com/Curryxin/p/15131151.html)   

[剑指 Offer 59 - II. 队列的最大值](https://www.cnblogs.com/Curryxin/p/15130459.html)

[42. 接雨水](https://www.cnblogs.com/Curryxin/p/15135917.html)

[496.下一个更大元素I](https://www.cnblogs.com/Curryxin/p/14777899.html)       

[503. 下一个更大元素 II](https://www.cnblogs.com/Curryxin/p/15136915.html)
 
[316. 去除重复字母](https://www.cnblogs.com/Curryxin/p/15136975.html)    

---

**大根堆或小根堆**   

[23. 合并K个升序链表](https://www.cnblogs.com/Curryxin/p/15133804.html)   

[剑指 Offer 40. 最小的k个数](https://www.cnblogs.com/Curryxin/p/15133578.html)    

[215. 数组中的第K个最大元素](https://www.cnblogs.com/Curryxin/p/15133554.html)
