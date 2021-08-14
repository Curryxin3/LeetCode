## 1.两数之和
> **知识点：数组，哈希表**；
### 题目描述
给定一个整数数组 nums 和一个整数目标值 target，请你在该数组中找出 和为目标值 target 的那两个整数，并返回它们的数组下标。

你可以假设每种输入只会对应一个答案。但是，数组中同一个元素在答案里不能重复出现。

你可以按任意顺序返回答案。

##### 示例

```
输入：nums = [2,7,11,15], target = 9
输出：[0,1]
解释：因为 nums[0] + nums[1] == 9 ，返回 [0, 1] 。

输入：nums = [3,2,4], target = 6
输出：[1,2]

输入：nums = [3,3], target = 6
输出：[0,1]
```
---

### 解法一：暴力法
直接进行两边数组的遍历；
```
class Solution {
    public int[] twoSum(int[] nums, int target) {
        int[] ans = new int[2];
        for (int i = 0; i < nums.length-1; i++){
            for(int j = i + 1; j < nums.length; j++){
                if (nums[i] + nums[j] == target){
                    ans[0] = i;
                    ans[1] = j;
                    return ans;
                }
            }
        }
        return null;
    }
}
```
时间复杂度：O(N^2);
### 解法二：哈希表
仔细想一下，我们上面的开销主要在于寻找target-nums[i]这个过程，这个过程需要将整个数组遍历；哈希表的话在寻找值的时候复杂度是O(1);所以可以用哈希表存起来，然后再在哈希表里查找；

```
class Solution {
    public int[] twoSum(int[] nums, int target) {
        HashMap<Integer,Integer> map = new HashMap<>();
        for (int i = 0; i < nums.length; i++){
            //检验是否存在期望的值；存在直接返回；这个过程的复杂度是O(1);
            //notes：以后刷题不要新建数组，直接在最后的返回时匿名对象就可以了；
            if (map.containsKey(target - nums[i])) return new int[]{map.get(target - nums[i]), i};
            //不存在加入到哈希表中；
            else map.put(nums[i], i);
        }
        return null;
    }
}
```
时间复杂度：O(N);   
空间复杂度：O(N);哈希表的开销；
### 体会
其实我们经常会用到哈希表的containsKey()的这个方法；因为它的检索时间复杂度为常数；
