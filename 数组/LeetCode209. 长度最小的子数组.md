## 209. 长度最小的子数组
> **知识点：数组；前缀和；二分查找；双指针；滑动窗口**
### 题目描述

给定一个含有 n 个正整数的数组和一个正整数 target 。

找出该数组中满足其和 ≥ target 的长度最小的 连续子数组 [numsl, numsl+1, ..., numsr-1, numsr] ，并返回其长度。如果不存在符合条件的子数组，返回 0 。

##### 示例
```
输入：target = 7, nums = [2,3,1,2,4,3]
输出：2
解释：子数组 [4,3] 是该条件下的长度最小的子数组。

输入：target = 4, nums = [1,4,4]
输出：1

输入：target = 11, nums = [1,1,1,1,1,1,1,1]
输出：0
```
---
### 解法一：暴力法

使用两层for循环遍历,找到最大子长度；
```
class Solution {
    public int minSubArrayLen(int target, int[] nums) {
        int ans = Integer.MAX_VALUE;
        for(int i = 0; i < nums.length; i++){
            int sum = 0;
            for(int j = i; j < nums.length; j++){
                sum += nums[j];
                if(sum >= target){
                    ans = Math.min(ans, j-i+1);
                    break;
                }
            }
        }
        return ans == Integer.MAX_VALUE ? 0 : ans;
    }
}
```
时间复杂度:0(N^2);

### 解法二：前缀和+二分查找

这道题一看连续子数组触发了**前缀和**，前缀和就是用来解决连续子数组问题的，所以可以先统计到达每个的前缀和，仍然是为了方便，创建长度是n+1的数组，第一个位置放0，这样最后输出子数组长度的时候不用再去+1了，直接j-i就可以了。    
前缀和之后，要想寻找子数组和大于target，就是找sum[i]-sum[j] >= target, 仍然需要遍历两遍，但是前缀和数组有一个重要的特点，就是是递增的(因为只有正数),所以这时候可以用**二分搜索**，将复杂度降低到O(logN),我们就是想找大于等于sum[j]+target的值.    
**在java中Arrays类有可以直接调用的Arrays.binarySearch(数组，目标值)；如果能找到，就返回找到值的下标，如果没找到就返回一个负数，这个负数取反之后就是查找的值应该在原数组中的位置。**

```
class Solution {
    public int minSubArrayLen(int target, int[] nums) {
        int minLen = Integer.MAX_VALUE;
        int[] presum = new int[nums.length+1]; //第一位放0；
        for(int i = 1; i < presum.length; i++){
            presum[i] = presum[i-1]+nums[i-1];  //前缀和数组；表示到i之前的所有和，不包括i
        }
        for(int i = 0; i < nums.length; i++){
            int t = target+presum[i];
            int index = Arrays.binarySearch(presum, t);  //二分查找；
            if(index < 0) index = ~index;   //没找到的话就取反返回应该插入的位置；
            if(index <= nums.length){
                minLen = Math.min(minLen, index-i);
            }
        }
        return minLen == Integer.MAX_VALUE ? 0 : minLen;
    }
}
```
时间复杂度:0(NlogN);

### 解法三：滑动窗口

滑动窗口其实就是双指针的一种，只不过在移动过程中像是一个窗口在移动，所以称作滑动窗口，这是解决连续数组中一个很常见的思路；  

可以定义两个指针start和end(分别表示窗口的开始和结束)，end从头到尾去遍历数组，当移动出去之后遍历结束。在过程中，将nums[end]不断的加到sum上，如果sum>= target, 那就更新数组的最小长度，然后将nums[stat]在sum中减去，start前移，直到sum< target后，end再移；

```
class Solution {
    public int minSubArrayLen(int target, int[] nums) {
        int minLen = Integer.MAX_VALUE;
        int left= 0, right = 0;
        int sum = 0;
        while(right < nums.length){
            sum += nums[right];
            while(sum >= target){
                minLen = Math.min(minLen, right-left+1);
                sum -= nums[left]; //窗口移动，减去出窗口的值；
                left++;
            }
            right++;
        }
        return minLen == Integer.MAX_VALUE ? 0 : minLen;
    }
}
```

### 体会
连续子数组+和 --> 前缀和      
连续子串+最值 --> 滑动窗口

### 相关链接
[长度最小的子数组-看滑动窗口](https://leetcode-cn.com/problems/minimum-size-subarray-sum/solution/javade-jie-fa-ji-bai-liao-9985de-yong-hu-by-sdwwld/)    
[长度最小的子数组-看前缀和](https://leetcode-cn.com/problems/minimum-size-subarray-sum/solution/javade-jie-fa-ji-bai-liao-9985de-yong-hu-by-sdwwld/)    
