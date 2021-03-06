## 66. 加一
> **知识点：数组**；
### 题目描述

给定一个由 **整数** 组成的 **非空** 数组所表示的非负整数，在该数的基础上加一。
最高位数字存放在数组的首位， 数组中每个元素只存储**单个**数字。
你可以假设除了整数 0 之外，这个整数不会以零开头。
##### 示例
```
输入：digits = [1,2,3]
输出：[1,2,4]
解释：输入数组表示数字 123。

输入：digits = [4,3,2,1]
输出：[4,3,2,2]
解释：输入数组表示数字 4321。

输入：digits = [0]
输出：[1]
```
---
### 解法一：
这个题目很明显关键就在于最后一位，最后一位呢分为两种情况；

- 末尾数字<9 --> 直接将末尾+1并返回；
- 末尾数字=9：
  - 前一位数字<9 --> 将末尾数字置为0，前一位数字+1并返回；
  - 前一位数字=9 --> 继续看前一位，如果到头了也就是说明所有的数字都为9，数组扩大一位且最高位置1其余全为0；

```
class Solution {
    public int[] plusOne(int[] digits) {
        int l = digits.length;
        for(int i = l-1; i >= 0; i--){
            if (digits[i] < 9){
                digits[i]++;
                return digits;  //只要有小于9的就可以直接返回了；
            }
            digits[i] = 0;
        }
        //出了循环的就说明上面全为9；
        digits = new int[l+1]; 
        //注意这里不能去int[] digits = new ...;这样就重名了，只能拿这个变量指向一个新的堆；
        digits[0] = 1;
        return digits;
    }
}
```
时间复杂度：O(N);
