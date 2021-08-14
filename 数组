## 485.最大连续1的个数
> **知识点：数组；**
### 题目描述
给定一个二进制数组， 计算其中最大连续 1 的个数。
##### 示例

```
输入：[1,1,0,1,1,1]
输出：3
解释：开头的两位和最后的三位都是连续 1 ，所以最大连续 1 的个数是 3.
```
##### 提示
- 输入的数组只包含 0 和 1 。
- 输入数组的长度是正整数，且不超过 10,000。

---
### 解法一

```
class Solution {
    public int findMaxConsecutiveOnes(int[] nums) {
        int max = 0;
        int count = 0;
        for(int i = 0; i < nums.length; i++){
            if(nums[i] == 1) count++;
            else count = 0;
            max = (count > max) ? count : max;
        }
        return max;
    }
}
```
时间复杂度：O(N);
