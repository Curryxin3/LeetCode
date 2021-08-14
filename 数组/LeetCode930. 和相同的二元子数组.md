## 930. 和相同的二元子数组
> **知识点：数组；前缀和；**
### 题目描述

给你一个二元数组 nums ，和一个整数 goal ，请你统计并返回有多少个和为 goal 的 非空 子数组。
子数组 是数组的一段连续部分。

##### 示例
```
输入：nums = [1,0,1,0,1], goal = 2
输出：4
解释：
有 4 个满足题目要求的子数组：[1,0,1]、[1,0,1,0]、[0,1,0,1]、[1,0,1]

输入：nums = [0,0,0,0,0], goal = 0
输出：15。
```
---
### 解法一：前缀和
连续子数组+和 --> 前缀和；
```
class Solution {
    public int numSubarraysWithSum(int[] nums, int goal) {
        Map<Integer,Integer> map = new HashMap<>();
        int presum = 0;
        int count = 0;
        map.put(0,1);
        for(int i = 0; i < nums.length; i++){
            presum += nums[i];
            if(map.containsKey(presum-goal)) count += map.get(presum-goal);
            map.put(presum, map.getOrDefault(presum, 0)+1);
        }
        return count;
    }
}
```
时间复杂度:0(N);
### 体会
连续子数组+和 --> 前缀和
