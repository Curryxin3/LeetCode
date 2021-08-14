## 81. 搜索旋转排序数组 II

> **知识点：数组，二分查找**；
### 题目描述

已知存在一个按非降序排列的整数数组 nums ，数组中的值不必互不相同。

在传递给函数之前，nums 在预先未知的某个下标 k（0 <= k < nums.length）上进行了 旋转 ，使数组变为 [nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]]（下标 从 0 开始 计数）。例如， [0,1,2,4,4,4,5,6,6,7] 在下标 5 处经旋转后可能变为 [4,5,6,6,7,0,1,2,4,4] 。

给你 旋转后 的数组 nums 和一个整数 target ，请你编写一个函数来判断给定的目标值是否存在于数组中。如果 nums 中存在这个目标值 target ，则返回 true ，否则返回 false 。

##### 示例

```
输入：nums = [2,5,6,0,0,1,2], target = 0
输出：true

输入：nums = [2,5,6,0,0,1,2], target = 3
输出：false
```
---

### 解法一：二分查找

这道题目和33题唯一的区别就在于这道里数组元素不是唯一的，也就是有重复元素。这就会在之前的解法上产生一个问题，就是没有办法再根据mid的值判断哪部分是有序的了。比如   
10111 和 11101这种。此种情况下 nums[left] == nums[mid]，分不清到底是前面有序还是后面有序，此时只需要让left++ 即可。相当于去掉一个重复的干扰项。

```
class Solution {
    public boolean search(int[] nums, int target) {
        if(nums == null) return false;
        int left = 0, right = nums.length-1;
        while(left <= right){
            int mid = left + ((right-left) >> 1);
            if(nums[mid] == target) return true;
            if(nums[mid] == nums[left]) {
                left++;
                continue;  //去掉干扰项；
            }
            if(nums[mid] > nums[left]){
                if(target >= nums[left] && target < nums[mid]){
                    right = mid-1;
                }else{
                    left = mid+1;
                }
            }else if(nums[mid] < nums[left]){
                if(target > nums[mid] && target <= nums[right]){
                    left = mid+1;
                }else{
                    right = mid-1;
                }
            }
        }
        return false;
    }
}
```

### 相关链接  
[搜索旋转排序数组II](https://leetcode-cn.com/problems/search-in-rotated-sorted-array-ii/solution/zai-javazhong-ji-bai-liao-100de-yong-hu-by-reedfan/)

