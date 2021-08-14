## 64. 最小路径和
> **知识点：动态规划**
### 题目描述

给定一个包含非负整数的 m x n 网格 grid ，请找出一条从左上角到右下角的路径，使得路径上的数字总和为最小。

说明：每次只能向下或者向右移动一步。

##### 示例

![image](https://assets.leetcode.com/uploads/2020/11/05/minpath.jpg)

```
输入：grid = [[1,3,1],[1,5,1],[4,2,1]]
输出：7
解释：因为路径 1→3→1→1→1 的总和最小。

输入：grid = [[1,2,3],[4,5,6]]
输出：12
```
---
### 解法一：动态规划

- **1.确定dp数组和其下标的含义**；dp[i][j]表示走到i，j位置时的最小路径和；
- **2.确定递推公式，即状态转移方程**；题目中要求只能向右或者向下走，所以走到i，j的最小路径和只和其左边和上面有关，也就是这两个单元格的较小一个再加上grid[i][j];   要注意矩阵的左边界是没有左边单元格的，而上边界是没有上边单元格的，所以需要单独处理；左边界就是dp[i][0] = dp[i-1][0]+grid[i][0];上边界dp[0][i]=dp[0][i-1]+grid[0][i];
- **3.dp初始化**；base case; 从最左上角位置开始，dp[0][0]=grid[0][0];  

最后返回右下角元素即可：dp[row-1][col-1]

```
class Solution {
    public int minPathSum(int[][] grid) {
        int m = grid.length;
        int n = grid[0].length;
        int[][] dp = new int[m][n]; //到达[i,j]位置时的最小路径和；
        dp[0][0] = grid[0][0];
        for(int i = 1; i < m; i++){
            dp[i][0] = dp[i-1][0]+grid[i][0];
        }
        for(int j = 1; j < n; j++){
            dp[0][j] = dp[0][j-1]+grid[0][j];
        }
        for(int i = 1; i < m; i++){
            for(int j = 1; j < n; j++){
                dp[i][j] = Math.min(dp[i][j-1], dp[i-1][j])+grid[i][j];
            } 
        }
        return dp[m-1][n-1];
    }
}
```

