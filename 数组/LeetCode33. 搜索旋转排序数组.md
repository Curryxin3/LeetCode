## 33. 搜索旋转排序数组

> **知识点：数组，二分查找**；
### 题目描述

整数数组 nums 按升序排列，数组中的值 互不相同 。

在传递给函数之前，nums 在预先未知的某个下标 k（0 <= k < nums.length）上进行了 旋转，使数组变为 [nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]]（下标 从 0 开始 计数）。例如， [0,1,2,4,5,6,7] 在下标 3 处经旋转后可能变为 [4,5,6,7,0,1,2] 。

给你 旋转后 的数组 nums 和一个整数 target ，如果 nums 中存在这个目标值 target ，则返回它的下标，否则返回 -1 。

##### 示例

```
输入：nums = [4,5,6,7,0,1,2], target = 0
输出：4

输入：nums = [4,5,6,7,0,1,2], target = 3
输出：-1

输入：nums = [1], target = 0
输出：-1
```
---

### 解法一：二分查找

这是不完全有序数组的二分查找。   
不完全有序，也就是数组中的两个子数组是有序的。  
在这道题目里，因为数值的不同，所以第一个子数组的首一定大于第二个子树的尾。  
思路：两个划分  
- 1.根据mid和left的值判断mid和left在同一数组还是和right同一数组。比如同时在数组1和数组2。如果不在那只能是left在数组1，mid在数组2。  
- 2.根据target和mid的值判断target在mid的左还是右。然后来缩小范围，总原则就是将left和right往有序上赶。

1 2 3 4 5 6 7 可以大致分为两类，
- 第一类 2 3 4 5 6 7 1 这种，也就是 nums[start] <= nums[mid]。此例子中就是 2 <= 5。
这种情况下，前半部分有序。因此如果 nums[start] <=target<nums[mid]，则在前半部分找，否则去后半部分找。
- 第二类 6 7 1 2 3 4 5 这种，也就是 nums[start] > nums[mid]。此例子中就是 6 > 2。
这种情况下，后半部分有序。因此如果 nums[mid] <target<=nums[end]，则在后半部分找，否则去前半部分找。


```
class Solution {
    public int search(int[] nums, int target) {
        int left = 0, right = nums.length-1;
        while(left <= right){
            int mid = left + ((right-left) >> 1);
            if(nums[mid] == target) return mid;
            if(nums[mid] >= nums[left]){ //在同一数组，前半段有序；带等号因为可能left和mid指向同一元素；
                if(target >= nums[left] && target < nums[mid]){ //后者不带等号因为前面已经判断过了；
                    right = mid-1;  //完全有序的一段；
                }else{
                    left = mid+1;
                }
            }else if(nums[mid] < nums[left]){ //不在同一数组，后半段有序；
                if(target > nums[mid] && target <= nums[right]){
                    left = mid+1;   //完全有序的一段；
                }else{
                    right = mid-1;
                }
            }
        }
        return -1;
    }
}
```
### 体会
这种半有序数组其实就是想找出来哪段是有序的，因为我们的二分查找只有在有序的时候才能用；    
根据left和mid的值关系就能够得到前半段有序还是后半段有序；  
然后再根据target所处的位置：看其是否正好在有序的区间，如果在直接用二分查找就可以了，
不在的话就往有序的位置上赶。

### 相关链接  
[搜索旋转排序数组](https://leetcode-cn.com/problems/search-in-rotated-sorted-array/solution/ji-bai-liao-9983de-javayong-hu-by-reedfan/)

