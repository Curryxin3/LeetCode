## 322. 零钱兑换
> **知识点：动态规划**
### 题目描述

给你一个整数数组 coins ，表示不同面额的硬币；以及一个整数 amount ，表示总金额。

计算并返回可以凑成总金额所需的 最少的硬币个数 。如果没有任何一种硬币组合能组成总金额，返回 -1 。

你可以认为每种硬币的数量是无限的。

##### 示例

```
输入：coins = [1, 2, 5], amount = 11
输出：3 
解释：11 = 5 + 5 + 1：

输入：coins = [2], amount = 3
输出：-1

输入：coins = [1], amount = 0
输出：0

输入：coins = [1], amount = 1
输出：1

输入：coins = [1], amount = 2
输出：2
```
---
### 解法一：动态规划

- **1.确定dp数组和其下标的含义**；dp[i]表示金额为i时所需要的最少硬币个数；
- **2.确定递推公式，即状态转移方程**；有两种可能，1.有coin[i]，也就是说如果某个小于当前金额i的j加上一个硬币值coin就等于现在要求的金额，即j+coin=i，那dp[i] = dp[j] + 1;也就是金额为j时的最小值+这次的coin。2.不存在这一个coin面值，那要兑换的硬币数量就是dp[i];
- **3.dp初始化**；base case; 最上面的元素，也就是dp[0]=0；同时对其他dp填充为amount+1，因为这是不可能的情况，其实就相当于初始化为正无穷，便于后面取最小值；

```
class Solution {
    public int coinChange(int[] coins, int amount) {
        int[] dp = new int[amount+1];
        Arrays.fill(dp, amount+1); //dp初始化为正无穷；
        dp[0] = 0;
        for(int i = 1; i < amount+1; i++){
            for(int coin:coins){
                if(i - coin < 0){
                    continue;  //凑不出这个i，无解；
                }
                dp[i] = Math.min(dp[i], dp[i-coin]+1);  //状态转移；
            }
        }
        return (dp[amount] == amount+1) ? -1 : dp[amount];
    }
}
```
