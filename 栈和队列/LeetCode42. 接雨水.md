## 42. 接雨水
> **知识点：栈；单调**
### 题目描述

给定 n 个非负整数表示每个宽度为 1 的柱子的高度图，计算按此排列的柱子，下雨之后能接多少雨水。

##### 示例

![image](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/10/22/rainwatertrap.png)

```
输入：height = [0,1,0,2,1,0,1,3,2,1,2,1]
输出：6
解释：上面是由数组 [0,1,0,2,1,0,1,3,2,1,2,1] 表示的高度图，在这种情况下，可以接 6 个单位的雨水（蓝色部分表示雨水）。

输入：height = [4,2,0,3,2,5]
输出：9
```
---
### 解法一：单调

只有两边高中间低的时候才能去接雨水。所以可以维护一个单调递减的栈，这时候如果新来的元素小于等于栈顶，那就可以直接入栈，如果新来元素大于栈顶元素，因为栈顶下面的比栈顶大，新来的比栈顶大，这证明找到了能接雨水的地方。怎么求雨水呢，自然是宽*高，宽就是两堵墙之间的距离，也就是新来的和弹出后的新栈顶的索引距离，高度就是两堵墙之间较小的那个-弹出的高度，这就是能接水的容器的高度。     

这又是用了单调栈的思想。单调栈真的很常用。

```
class Solution {
    public int trap(int[] height) {
        int water = 0;
        Stack<Integer> stack = new Stack<>();  //维持一个单调递减栈；栈里存放下标；
        int index = 0;
        while(index < height.length){
            while(!stack.isEmpty() && height[index] > height[stack.peek()]){
                int h = height[stack.pop()]; //出栈的高度，也就是底部的高度；
                if(stack.isEmpty()){
                    break;   //没有另一边墙没法接水；
                }
                int distance = index - stack.peek() - 1; //容器的宽度；
                int min = Math.min(height[stack.peek()], height[index]);  //两堵墙哪个低；
                water = water + distance*(min-h);
            }
            stack.push(index);
            index++;
        }
        return water;
    }
}
```   

### 相关链接

[接雨水](https://leetcode-cn.com/problems/trapping-rain-water/solution/xiang-xi-tong-su-de-si-lu-fen-xi-duo-jie-fa-by-w-8/)
