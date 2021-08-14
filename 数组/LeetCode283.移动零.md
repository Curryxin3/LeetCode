## 283.移动零
> **知识点：数组；双指针；**
### 题目描述
给定一个数组 nums，编写一个函数将所有 0 移动到数组的末尾，同时保持非零元素的相对顺序。
##### 示例

```
输入: [0,1,0,3,12]
输出: [1,3,12,0,0]
```
##### 说明
- 必须在原数组上操作，不能拷贝额外的数组。
- 尽量减少操作次数。

---
### 解法一：冒泡排序思想
冒泡排序是只要当前元素比对方元素小，就交换两者，否则不动；
可以只要遇到了0，就交换两者向后移动；
```
class Solution {
    public void moveZeroes(int[] nums) {
        for(int i = 0; i < nums.length; i++){
            for(int j = 0; j < nums.length - 1 - i; j++){
                if(nums[j] == 0){
                    nums[j] = nums[j+1];
                    nums[j+1] = 0;
                }
            }
        }
    }
}
```
时间复杂度：O(N^2);
### 解法二：左双指针
再仔细想一下这道题，把所有的0移动到最后，那不就是把不是零的按照顺序依次填到开头的位置上，当把数组遍历一遍把非0的填完之后，再把剩下的填成0就可以了，
所以定义两个指针，一个负责遍历，一个负责指定非零元素插入的位置；
```
class Solution {
    public void moveZeroes(int[] nums) {
        int i = 0;
        int j = 0;
        for(; i < nums.length; i++){
            if(nums[i] != 0){
                nums[j] = nums[i];
                j++;
            }
        }
        for(; j < nums.length; j++){
            nums[j] = 0;
        }
    }
}
```
时间复杂度：O(N);
