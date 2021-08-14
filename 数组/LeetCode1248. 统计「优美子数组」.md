## 1248. 统计「优美子数组」
> **知识点：数组；前缀和**；
### 题目描述

给你一个整数数组 nums 和一个整数 k。

如果某个 连续 子数组中恰好有 k 个奇数数字，我们就认为这个子数组是「优美子数组」。

请返回这个数组中「优美子数组」的数目

##### 示例
```
输入：nums = [1,1,2,1,1], k = 3
输出：2
解释：包含 3 个奇数的子数组是 [1,1,2,1] 和 [1,2,1,1] 。

输入：nums = [2,4,6], k = 1
输出：0
解释：数列中不包含任何奇数，所以不存在优美子数组。

输入：nums = [2,2,2,1,2,2,1,2,2,2], k = 2
输出：16
```
---
### 解法一：前缀和
又是连续子序列，想前缀和，和560题：和为k的子数组有区别吗？其实是一模一样的。前者是子数组和为k，现在是子数组奇数个数为k，换汤不换药；直接前缀和+哈希表。
```
class Solution {
    public int numberOfSubarrays(int[] nums, int k) {
        Map<Integer,Integer> map = new HashMap<>();
        map.put(0,1);
        int count = 0;
        int presum = 0;
        for(int i = 0; i < nums.length; i++){
            presum += nums[i] % 2;   //统计奇数个数前缀和；
            if(map.containsKey(presum-k)) count += map.get(presum-k);
            map.put(presum, map.getOrDefault(presum, 0)+1);
        }
        return count;
    }
}
```
时间复杂度：O(N);
### 体会
连续子数组，想前缀和。
