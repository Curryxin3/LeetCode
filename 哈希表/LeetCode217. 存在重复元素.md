## 217. 存在重复元素
> **知识点：数组,Hashmap, Set；**
### 题目描述 

给定一个整数数组，判断是否存在重复元素。
如果存在一值在数组中出现至少两次，函数返回 true 。如果数组中每个元素都不相同，则返回 false 。

##### 示例
```
示例1：
输入: [1,2,3,1]
输出: true
示例2：
输入: [1,2,3,4]
输出: false
示例3：
输入: [1,1,1,3,3,4,3,2,4,2]
输出: true
```
---
### 解法一：数组排序
其实可以直接对数组进行排序，然后直接对比前后两个元素即可，因为如果有一样的元素，两个元素一定相邻。
```
class Solution {
    public boolean containsDuplicate(int[] nums) {
        Arrays.sort(nums);
        for(int i = 0; i<nums.length-1; i++){
            if(nums[i] == nums[i+1]) return true;
        }
        return false;
    }
}
```
### 解法二：HashSet
重复元素，首先应该想到的就是HashSet，因为其特点之一就是不可重复；而且其有一个重要的方法：contains();
```
class Solution {
    public boolean containsDuplicate(int[] nums) {
        Set<Integer> hashSet = new HashSet<>();
        for(int i : nums){
            if(hashSet.contains(i)) return true;
            hashSet.add(i);
        }
        return false;
    }
}
```
以上两种方法也都是遍历一遍数组；
### 体会
最重要的就是hashset的不可重复性；
数组和HashSet的主要方法；
