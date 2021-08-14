## 523. 连续的子数组和
> **知识点：数组；前缀和；**
### 题目描述

给你一个整数数组 nums 和一个整数 k ，编写一个函数来判断该数组是否含有同时满足下述条件的连续子数组：

子数组大小 至少为 2 ，且
子数组元素总和为 k 的倍数。
如果存在，返回 true ；否则，返回 false 。

如果存在一个整数 n ，令整数 x 符合 x = n * k ，则称 x 是 k 的一个倍数。0 始终视为 k 的一个倍数。

##### 示例
```
输入：nums = [23,2,4,6,7], k = 6
输出：true
解释：[2,4] 是一个大小为 2 的子数组，并且和为 6 。

输入：nums = [23,2,6,4,7], k = 6
输出：true
解释：[23, 2, 6, 4, 7] 是大小为 5 的子数组，并且和为 42 。 
42 是 6 的倍数，因为 42 = 7 * 6 且 7 是一个整数。

输入：nums = [23,2,6,4,7], k = 13
输出：false
```
---
### 解法一：前缀和
连续子数组+和 --> 前缀和；这个题是要我们返回是否存在，比较的是子数组大小，那我们的value值就不能再是值为某数的次数了，而是元素的索引，这样才能得到其大小；
```
class Solution {
    public boolean checkSubarraySum(int[] nums, int k) {
        Map<Integer,Integer> map = new HashMap<>();
        map.put(0,-1);
        int presum = 0;
        for(int i = 0; i < nums.length; i++){
            presum += nums[i];
            int mod = (presum % k + k) % k;  //余数调整；
            if(map.containsKey(mod)){
                if(i - map.get(mod) >= 2) return true;
                continue;    
                //细节，如果长度没有超过2，但是已经有这个余数了，就跳出此次循环；
                //因为我们要保存最小的索引，不能在这里更新；
            }
            map.put(mod, i);
        }
        return false;
    }
}
```
时间复杂度:0(N);
### 体会
连续子数组+和 --> 前缀和
