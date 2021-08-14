## 70. 爬楼梯
> **知识点：动态规划**
### 题目描述

假设你正在爬楼梯。需要 n 阶你才能到达楼顶。

每次你可以爬 1 或 2 个台阶。你有多少种不同的方法可以爬到楼顶呢？

注意：给定 n 是一个正整数。

##### 示例

```
输入： 2
输出： 2
解释： 有两种方法可以爬到楼顶。
1.  1 阶 + 1 阶
2.  2 阶

输入： 3
输出： 3
解释： 有三种方法可以爬到楼顶。
1.  1 阶 + 1 阶 + 1 阶
2.  1 阶 + 2 阶
3.  2 阶 + 1 阶
```
---
### 解法一：动态规划

我们先创建dp数组，首先     
- **1.确定dp数组和其下标的含义**；(爬到第i层楼梯有dp[i]种方法)
- **2.确定递推公式，即状态转移方程**；(我们每次都只能爬一层或者爬两层，那如果要爬到第i层，这个状态只能由第i-1或者i-2得到，所以dp[i] = dp[i-1]+dp[i-2]) 
- **3.dp初始化**；base case; (题目中给出了n为正整数，所以dp[1]=1,dp[2]=2)

```
class Solution {
    public int climbStairs(int n) {
        if(n <= 1) return n;
        int dp[] = new int[n+1];  //到达第i层楼梯的方法有dp[i]种；
        dp[1] = 1;
        dp[2] = 2;
        if(n <= 2) return dp[n];
        for(int i = 3; i < n+1; i++){
            dp[i] = dp[i-1]+dp[i-2];  //递推关系：状态转移方程；
        }
        return dp[n];
    }
}
```
优化后的：

```
class Solution {
    public int climbStairs(int n) {
        if(n <= 1) return n;
        int pre = 1; int cur = 2;
        for(int i = 3; i < n+1; i++){
            int sum = pre+cur;
            pre = cur;
            cur = sum;
        }
        return cur;
    }
}
```

### 体会

其实本题和509题斐波那契数列是一样的，但是为什么这个题感觉就难很多呢，最主要的问题就是题目中没有把状态转移方程和dp初始化给出来，我们需要自己去发现，这就是题的难度所在，如果都直接给你了是不是也就会写了。

### 相关链接
[代码随想录](https://leetcode-cn.com/problems/fibonacci-number/solution/dai-ma-sui-xiang-lu-509-fei-bo-na-qi-shu-n389/)
