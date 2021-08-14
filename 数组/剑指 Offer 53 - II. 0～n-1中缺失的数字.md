## 剑指 Offer 53 - II. 0～n-1中缺失的数字

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

看到排序数组--> 二分查找；    
根据题目：可以根据是否和索引对应分成两部分：  
- 1.nums[i] == i; 索引和值能一一对应上；  
- 2.nums[i] != i; 索引和值对不上了；  

我们要找的就是第一个对不上的时候的索引；也就是右子数组的首位元素；   

和33题一样，要明确的一点是最后执行的一定是left=right=mid，而且此时mid左侧都是对应的，mid右侧都是不对应的，所以判断此刻mid的值：  
- 1.nums[mid] == mid; 那left=mid+1就是第一个不匹配的；  
- 2.nums[mid] != mid，那left就是第一个不匹配的；  
所以最后返回left就可以了。

```
class Solution {
    public int missingNumber(int[] nums) {
        int left = 0, right = nums.length-1;
        while(left <= right){
            int mid = left + ((right-left) >> 1);
            if(nums[mid] == mid){
                left = mid+1; //一定在右边；
            }else{
                right = mid-1; //可能正好是mid，但是我们还是可以mid-1，因为最后如果相等了，我们返回的是left；
                //在上面执行了+1的操作，所以正好是不等的。要始终明白在最后时刻的时候，left=mid=right，而且mid左边都排好了，mid右边都乱了，只要看mid这个就行了；
                //（注意和153题区分，那里就不能减1）；
            }
        }
        return left;
    }
}
```


