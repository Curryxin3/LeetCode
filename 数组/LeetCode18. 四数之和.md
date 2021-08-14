## 18. 四数之和

> **知识点：数组，双指针**；
### 题目描述

给定一个包含 n 个整数的数组 nums 和一个目标值 target，判断 nums 中是否存在四个元素 a，b，c 和 d ，使得 a + b + c + d 的值与 target 相等？找出所有满足条件且不重复的四元组。

注意：答案中不可以包含重复的四元组。

##### 示例

```
输入：nums = [1,0,-1,0,-2,2], target = 0
输出：[[-2,-1,1,2],[-2,0,0,2],[-1,0,0,1]]

输入：nums = [], target = 0
输出：[]

输入：nums = [0]
输出：[]
```
---

### 解法一：双指针

此题和15题三数之和一样，只不过又多了一层循环，先固定一个数字a，再固定后面一个数字b，然后开始双指针找，完事之后，再去移动b，重复，等b移动完了，再去移动a。

```
class Solution {
    public List<List<Integer>> fourSum(int[] nums, int target) {
        List<List<Integer>> list = new ArrayList<>();
        if(nums == null || nums.length <= 3) return list;
        Arrays.sort(nums);
        for(int i = 0; i < nums.length; i++){
            if(i > 0 && nums[i] == nums[i-1]) continue; //去重；
            for(int j = i+1; j < nums.length; j++){
                if(j > i+1 && nums[j] == nums[j-1]) continue;
                int left = j+1, right = nums.length-1;
                int t = target-nums[i]-nums[j];
                while(left < right){
                    if(nums[left] + nums[right] == t){
                        list.add(Arrays.asList(nums[i], nums[j], nums[left], nums[right]));
                        left++;
                        right--;
                        while(left < right && nums[left] == nums[left-1]) left++;
                        while(left < right && nums[right] == nums[right+1]) right--;
                    }else if(nums[left] + nums[right] < t){
                        left++;
                    }else{
                        right--;
                    }
                }
            }
        }
        return list;
    }
}
```
时间复杂度：O(N^3);

