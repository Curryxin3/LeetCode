## 120. 三角形最小路径和
> **知识点：动态规划；最小路径**
### 题目描述

给定一个三角形 triangle ，找出自顶向下的最小路径和。

每一步只能移动到下一行中相邻的结点上。相邻的结点 在这里指的是 下标 与 上一层结点下标 相同或者等于 上一层结点下标 + 1 的两个结点。也就是说，如果正位于当前行的下标 i ，那么下一步可以移动到下一行的下标 i 或 i + 1 。

##### 示例

```
输入：triangle = [[2],[3,4],[6,5,7],[4,1,8,3]]
输出：11
解释：如下面简图所示：
   2
  3 4
 6 5 7
4 1 8 3
自顶向下的最小路径和为 11（即，2 + 3 + 5 + 1 = 11）。

输入：triangle = [[-10]]
输出：-10
```
---
### 解法一：动态规划：自顶向下

- **1.确定dp数组和其下标的含义**；dp[i][j]走到位置[i,j]处的最小路径和；
- **2.确定递推公式，即状态转移方程**；由于到达位置[i,j]处只能由其上方或者其上方的左边到达，所以dp[i][j] = Math.min(dp[i-1][j-1], dp[i-1][j])+nums[i][j]; 但是需要注意，最左侧一列是没有左边值的，包括每行最后一位是没有上面值的，所以需要单独拿出来填充；
- **3.dp初始化**；base case; 最上面的元素，也就是dp[0][0]=nums[0][0]；   

```
class Solution {
    public int minimumTotal(List<List<Integer>> triangle) {
        int m = triangle.size();
        int[][] dp = new int[m][m]; //[i,j]处的最小路径和；
        dp[0][0] = triangle.get(0).get(0);
        for(int i = 1; i < m; i++){
            dp[i][0] = dp[i-1][0] + triangle.get(i).get(0);  //最左侧列；
            for(int j = 1; j < i; j++){
                dp[i][j] = Math.min(dp[i-1][j-1], dp[i-1][j]) + triangle.get(i).get(j);
            }
            dp[i][i] = dp[i-1][i-1] + triangle.get(i).get(i);  //边界；
        }
        int min = dp[m-1][0]; 
        for(int i = 1; i < m; i++){  //找到最小的；
            min = Math.min(min, dp[m-1][i]);
        }
        return min;
    }
}
```
### 解法二：动态规划：自底向上  

反过来，我们可以从下往上去构建dp数组，这样在最后的时候就直接返回dp[0][0]就可以了，而且不需要考虑边界条件；
   
```
class Solution {
    public int minimumTotal(List<List<Integer>> triangle) {
        int n = triangle.size();
        int[][] dp = new int[n+1][n+1];  //多建一行是为了最底下全为0；
        for(int i = n-1; i >= 0; i--){
            for(int j = 0; j <= i; j++){
                dp[i][j] = Math.min(dp[i+1][j], dp[i+1][j+1])+triangle.get(i).get(j);
            }
        }
        return dp[0][0];
    }
}
```

