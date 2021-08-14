## 59.螺旋矩阵II
> **知识点：数组**；
### 题目描述

给你一个正整数 n ，生成一个包含 1 到 n2 所有元素，且元素按顺时针顺序螺旋排列的 n x n 正方形矩阵 matrix 。
##### 示例
```
输入：n = 3
输出：[[1,2,3],[8,9,4],[7,6,5]]

输入：n = 1
输出：[[1]]
```
---
### 解法一：
和54题解法基本一致，利用四个边界条件进行遍历；

```
class Solution {
    public int[][] generateMatrix(int n) {
        int[][] ans = new int[n][n];
        int num = 1;
        int top = 0, down = n-1;
        int left = 0, right = n-1;
        while (true){
            for (int i = left; i <= right; i++){
                ans[top][i] = num;
                num++;
            }
            top++;
            if (top > down) break;
            for (int i = top; i <= down; i++){
                ans[i][right] = num;
                num++;
            }
            right--;
            if (left > right) break;
            for (int i = right; i >= left; i--){
                ans[down][i] = num;
                num++;
            }
            down--;
            if (top > down) break;
            for (int i = down; i >= top; i--){
                ans[i][left] = num;
                num++;
            }
            left++;
            if (left > right) break;
        }
        return ans;
    }
}
```
时间复杂度：O(N)；
