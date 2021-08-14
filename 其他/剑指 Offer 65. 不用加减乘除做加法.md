## 剑指 Offer 65. 不用加减乘除做加法
> **知识点：数学；位运算**

### 题目描述

写一个函数，求两个整数之和，要求在函数体内不得使用 “+”、“-”、“*”、“/” 四则运算符号。

##### 示例

```
输入: a = 1, b = 1
输出: 2
```
---
### 解法一：位运算   

不能用四则运算，那其实可以用的只要逻辑运算和位运算了，这里很明显要用位运算。关键怎么用呢。    

我们列一张表其实就可以发现，两个元素如果没有进位也就是不是对应位都是1的时候，相加就和异或运算的结果是一样的；如果有进位，也就是两位上都是1的时候，相加就是两者相与然后左移一位；所以我们就可以做了。     

可以依次得到两个数的异或结果和与结果；  
sum=a+b=sum+carry；但是求得这两个后还是要加，由于不能用加法，所以可以再求这两个结果的与结果和异或结果。直到最后进位的为0，sum就是答案了。

```
class Solution {
    public int add(int a, int b) {
        int sum = a^b;  //无进位的；
        int carry = (a&b) << 1;  //有进位的；
        while(carry != 0){  //进位为0的时候返回sum；
            a = sum;
            b = carry;
            sum = a^b;
            carry = (a & b) << 1;
        }
        return sum;   
    }
}
```

### 相关链接  

[不用加减乘除做加法（位运算，清晰图解）](https://leetcode-cn.com/problems/bu-yong-jia-jian-cheng-chu-zuo-jia-fa-lcof/solution/mian-shi-ti-65-bu-yong-jia-jian-cheng-chu-zuo-ji-7/)
