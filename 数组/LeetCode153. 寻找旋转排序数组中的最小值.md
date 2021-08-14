## 153. 寻找旋转排序数组中的最小值

> **知识点：数组，二分查找**；
### 题目描述

已知一个长度为 n 的数组，预先按照升序排列，经由 1 到 n 次 旋转 后，得到输入数组。例如，原数组 nums = [0,1,2,4,5,6,7] 在变化后可能得到：
若旋转 4 次，则可以得到 [4,5,6,7,0,1,2]
若旋转 7 次，则可以得到 [0,1,2,4,5,6,7]
注意，数组 [a[0], a[1], a[2], ..., a[n-1]] 旋转一次 的结果为数组 [a[n-1], a[0], a[1], a[2], ..., a[n-2]] 。

给你一个元素值 互不相同 的数组 nums ，它原来是一个升序排列的数组，并按上述情形进行了多次旋转。请你找出并返回数组中的 最小元素 。

##### 示例

```
输入：nums = [3,4,5,1,2]
输出：1
解释：原数组为 [1,2,3,4,5] ，旋转 3 次得到输入数组。

输入：nums = [4,5,6,7,0,1,2]
输出：0
解释：原数组为 [0,1,2,4,5,6,7] ，旋转 4 次得到输入数组。
```
---

### 解法一：二分查找

需要在一个旋转后的数组中查找最小值，如果是完全有序的话，那第一个值就是了，所以我们可以逐步靠近直到最后有序，那就返回left。   
- 1.如果left和mid都在前半数组里，那最小值一定在后半数组，移动left，left=mid+1；
- 2.如果left在前半段，mid在后半段，那最小值一定在两者之间，移动right，right=mid；(注意这时候不能-1，因为最小值万一就在mid上呢，减1的话就给漏掉了)。

```
class Solution {
    public int findMin(int[] nums) {
        int left = 0, right = nums.length-1;
        while(left <= right){
            if(nums[right] >= nums[left]) return nums[left];  //排序好的；
            int mid = left + ((right-left) >> 1);
            if(nums[mid] >= nums[left]){ //比如最简单的[2,1];
                left = mid+1;
            }else if(nums[mid] < nums[left]){
                right = mid;  //注意这里不需要-1，因为可能最小值就是mid；
            }
        }
        return -1;
    }
}
```

### 相关链接  
[旋转数组中的最小值](https://leetcode-cn.com/problems/search-insert-position/solution/yi-wen-dai-ni-gao-ding-er-fen-cha-zhao-j-69ao/)

