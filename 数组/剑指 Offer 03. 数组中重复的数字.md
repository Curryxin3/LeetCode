## 剑指 Offer 03. 数组中重复的数字
> **知识点：数组；哈希表；萝卜占坑思想**
### 题目描述
找出数组中重复的数字。


在一个长度为 n 的数组 nums 里的所有数字都在 0～n-1 的范围内。数组中某些数字是重复的，但不知道有几个数字重复了，也不知道每个数字重复了几次。请找出数组中任意一个重复的数字。

##### 示例
```
输入：
[2, 3, 1, 0, 2, 5, 3]
输出：2 或 3 

```
---
### 解法一：暴力法

```
class Solution {
    public int findRepeatNumber(int[] nums) {
        for(int i = 0; i < nums.length-1; i++){
            for(int j = i+1; j < nums.length; j++){
                if(nums[i] == nums[j]) return nums[i];
            }
        }
        return -1;
    }
}
```
时间复杂度:O(N^N);
### 解法二：数组排序

```
class Solution {
    public int findRepeatNumber(int[] nums) {
        Arrays.sort(nums);
        for(int i = 0; i < nums.length-1; i++){
            if(nums[i] == nums[i+1]){
                return nums[i];
            }
        }
        return -1;
    }
}
```
时间复杂度：O(N)+O(NlogN);
### 解法三：哈希表
利用哈希表不重复的性质，如果包括就把其返回，如果不重复就把其加入哈希表；
```
class Solution {
    public int findRepeatNumber(int[] nums) {
        Set<Integer> set = new HashSet<>();
        for(int i = 0; i < nums.length; i++){
            if(set.contains(nums[i])) return nums[i];
            else set.add(nums[i]);
        }
        return -1;
    }
}
```
时间复杂度：O(N);
空间复杂度：O(N);构建哈希表的消耗；
### 解法四：一个萝卜一个坑
充分考虑到题目中的数字范围，正好是0~n-1；所以可以依次将它们归位，如果归位过程中那个坑里有了，那就证明这个重复了；
```
class Solution {
    public int findRepeatNumber(int[] nums) {
        int temp;
        for (int i = 0; i < nums.length; i++){
            while (nums[i] != i){   //i这个位置没归位就一直找下去；
                if (nums[i] == nums[nums[i]]) return nums[i];
                temp = nums[i];
                nums[i] = nums[temp];
                nums[temp] = temp;
            }
        }
        return -1;
    }
}
```
时间复杂度：0(N); 
### 体会
一定要用集合的不可重复性；





