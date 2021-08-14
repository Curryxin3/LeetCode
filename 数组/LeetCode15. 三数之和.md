## 15. 三数之和

> **知识点：数组，双指针**；
### 题目描述

给你一个包含 n 个整数的数组 nums，判断 nums 中是否存在三个元素 a，b，c ，使得 a + b + c = 0 ？请你找出所有和为 0 且不重复的三元组。

注意：答案中不可以包含重复的三元组。

##### 示例

```
输入：nums = [-1,0,1,2,-1,-4]
输出：[[-1,-1,2],[-1,0,1]]

输入：nums = []
输出：[]

输入：nums = [0]
输出：[]
```
---

### 解法一：双指针

三数之和，直接用暴力法解决的话时间复杂度是O(N^3),
所以可以用双指针来优化，时间复杂度变为O(N^2).此外这道题目还有一个关键点就是去重，因为不能够有重复的。  
首先固定一个元素，然后-nums[i]就是要寻找的另外两个元素之和，首先对原数组进行排序，把重复的值集中到一起，方便去重，也方便双指针的移动(两数之和比目标值大了，移动右边减小，比目标值小了，移动左边变大)。   
确定元素时，如果发现它与前面的值一样了，可以直接跳过这个元素，如 [-1, -1, 0, 1], 在第一轮后，已经选出了 {-1, 0, 1}, 现在 i = 1，nums[i] == nums[i - 1], 为了避免重复，直接 continue。  
剩下的交给双指针就可以了。当然在这个过程中也要注意重复，比如[-1,-1,2,3,3];如果已经选中了-1和3，那后面的-1和3都要直接跳过。

```
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        List<List<Integer>> list = new ArrayList<>();
        if(nums == null || nums.length <= 2) return list;
        //排序方便去重；
        Arrays.sort(nums);
        for(int i = 0; i < nums.length-2; i++){
            if(nums[i] > 0) break; //不可能情况；
            if(i > 0 && nums[i] == nums[i-1]) continue; //相等的话直接跳过；
            //有序数组可以用首尾双指针；
            int target = -nums[i];
            int left = i+1, right = nums.length-1;
            while(left < right){
                if(nums[left] + nums[right] == target){
                    list.add(Arrays.asList(nums[i], nums[left], nums[right]));
                    left++;
                    right--;
                    //相同的直接跳过，防止重复；
                    while(left < right && nums[left] == nums[left-1]) left++;
                    while(left < right && nums[right] == nums[right+1]) right--;
                }else if(nums[left] + nums[right] > target){
                    right--;
                }else{
                    left++;
                }
            }
        }
        return list;
    }
}
```
时间复杂度：O(N^2);

### 相关题目
[1. 两数之和](https://www.cnblogs.com/Curryxin/p/15004061.html)    
[18. 四数之和](https://www.cnblogs.com/Curryxin/p/15100834.html)


### 相关链接

[三数之和](https://blog.csdn.net/starflyyy/article/details/106955473)    
