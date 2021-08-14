## 54. 螺旋矩阵
> **知识点：数组**；
### 题目描述

给你一个 m 行 n 列的矩阵 matrix ，请按照 顺时针螺旋顺序 ，返回矩阵中的所有元素。
##### 示例:
```
输入：matrix = [[1,2,3],[4,5,6],[7,8,9]]
输出：[1,2,3,6,9,8,7,4,5]
```

```
输入：matrix = [[1,2,3,4],[5,6,7,8],[9,10,11,12]]
输出：[1,2,3,4,8,12,11,10,9,5,6,7]
```
---
### 解法一：
可以把上下左右四个边界设置出来，然后按照顺时针顺序，每次一个边界完成之后就缩小边界的值，然后当上边界大于下边界，或者左边界大于右边界的时候就可以结束了；

```
class Solution {
    public List<Integer> spiralOrder(int[][] matrix) {
        List<Integer> ans = new ArrayList<>();
        int left = 0, right = matrix[0].length-1;
        int top = 0, down = matrix.length-1;
        while (true){
            for (int i = left; i <= right;i++){
                ans.add(matrix[top][i]);
            }
            top++;
            if (top > down) break;
            for(int i = top; i <= down; i++){
                ans.add(matrix[i][right]);
            }
            right--;
            if (left > right) break;
            for(int i = right; i >= left; i--){
                ans.add(matrix[down][i]);
            }
            down--;
            if (top > down) break;
            for(int i = down; i >= top; i--){
                ans.add(matrix[i][left]);
            }
            left++;
            if (left > right) break;
        }
        return ans;
    }
}
```
时间复杂度：O(N);
