## 74. 搜索二维矩阵

> **知识点：数组，二分查找**；

### 题目描述

编写一个高效的算法来判断 m x n 矩阵中，是否存在一个目标值。该矩阵具有如下特性：

每行中的整数从左到右按升序排列。
每行的第一个整数大于前一行的最后一个整数。

##### 示例

[link](https://note.youdao.com/yws/public/resource/9a28193678edcdcb18a7cd937c45da85/xmlnote/AD5925B1F0EE4DDC9171126DD3E3CD75/12377)
```
输入：matrix = [[1,3,5,7],[10,11,16,20],[23,30,34,60]], target = 3
输出：true

输入：matrix = [[1,3,5,7],[10,11,16,20],[23,30,34,60]], target = 13
输出：false
```
---

### 解法一：二分查找

这个二维数组是是有序的，想象一下，将其扁平化后就是一个有序的一维数组，就能使用二分查找了，所以可以想象一个一维数组，仍然和原来一样不断求中间值，只不过我们将中间值转化为二维数组的索引就可以了，
- 行坐标=mid/列数。比如5/4=1，就在第1行；
- 列坐标=mid%列数。比如5%4=1，就在第1列；   
这样就获得了其在二维数组中的值，nums[行坐标][列坐标]；然后判断其与target的值，进行移动就可以了。

[link](https://note.youdao.com/yws/public/resource/9a28193678edcdcb18a7cd937c45da85/xmlnote/6337C4AB5EC84BD4A6FCBD563512D49A/12405)

```
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        int m = matrix.length, n = matrix[0].length;
        int left = 0, right = m*n-1;
        while(left <= right){
            int mid = left + ((right-left) >> 1);
            int x = mid / n;
            int y = mid % n;
            if(matrix[x][y] == target){
                return true;
            }else if(matrix[x][y] > target){
                right = mid-1;
            }else{
                left = mid+1;
            }
        }
        return false;
    }
}
```

### 相关链接  
[搜索二维矩阵](https://leetcode-cn.com/problems/search-insert-position/solution/yi-wen-dai-ni-gao-ding-er-fen-cha-zhao-j-69ao/)

