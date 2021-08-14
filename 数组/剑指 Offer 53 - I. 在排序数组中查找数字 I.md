## 剑指 Offer 53 - I. 在排序数组中查找数字 I

> **知识点：数组，二分查找**；
### 题目描述

统计一个数字在排序数组中出现的次数。

##### 示例

```
输入: nums = [5,7,7,8,8,10], target = 8
输出: 2

输入: nums = [5,7,7,8,8,10], target = 6
输出: 0
```
---

### 解法一：二分查找

这道题和34题是一样的，其实就是在找左右边界，然后可以用right-left+1就是出现次数了。当然在程序中可以减少这种冗余，可以在找到左边界后，寻找target+1的位置，这样直接right-left就是最后的长度了。

**总的来说(关键)**：
- 左边界其实就是在找第一个>=target的位置  
   - 如果数组中有target，返回就是第一个target的位置；
   - 如果数组中无target，返回就是第一个比target大的元素是位置(或者可以理解成要插入target的位置)；
- 右边界其实就是在找最后一个<=target的位置
   - 如果数组中有target，返回就是最后target的位置；
   - 如果数组中无target，返回就是最后一个小于target的位置(或者可以理解成要插入的target的位置的前一个位置)。

所以最后就可以直接比较left和right的位置了，如果left比right还大，那证明不存在了。

可以直接看[34题](https://www.cnblogs.com/Curryxin/p/15101136.html)，求出左右边界，两道题一模一样，只有最后返回不一样。    

当然我们也可以转变思路，因为求左边界就是求出第一个大于等于target的值，所以可以求一个比target大的值的左边界，然后两者相减就是了。
```
class Solution {
    public int search(int[] nums, int target) {
        int leftbound = leftBound(nums,target);
        int rightbound = leftBound(nums, target+1); //求target+1的左边界；
        return rightbound-leftbound;
        
    }
    private int leftBound(int[] nums, int target){
        int left = 0, right = nums.length-1;
        while(left <= right){
            int mid = left + ((right-left) >> 1);
            if(target <= nums[mid]){
                right = mid-1;
            }else if(target > nums[mid]){
                left = mid+1;
            }
        }
        return left; 
    }
}
```


