## 169. 多数元素

> **知识点：数组；排序；消消乐；分治**；
### 题目描述

给定一个大小为 n 的数组，找到其中的多数元素。多数元素是指在数组中出现次数 大于 ⌊ n/2 ⌋ 的元素。

你可以假设数组是非空的，并且给定的数组总是存在多数元素。

##### 示例

```
输入：[3,2,3]
输出：3

输入：[2,2,1,1,1,2,2]
输出：2
```
---

### 解法一：排序

因为这道题中说了一定有一个数字是大于一半的，所以可以直接将数组进行排序，然后中间那个数一定是出现超过一半的。

```
class Solution {
    public int majorityElement(int[] nums) {
        if(nums.length == 1) return nums[0];
        Arrays.sort(nums);
        return nums[nums.length/2];
    }
}
``` 

### 解法二：消消乐(投票法)    

一定有一个数字超过一半，所以如果两个不同的数字两两抵消的话最后留下来的那个一定就是出现次数超过一半的。先选最开始的作为候选人，然后count=1， 如果后面遇到一样的，count++，遇到不一样的，count--，count一旦到0以后，那就需要换新的候选人了。

```
class Solution {
    public int majorityElement(int[] nums) {
        int res = 0, count = 0;
        for(int i = 0; i < nums.length; i++){
            if(count == 0){
                res = nums[i];
                count++;
            }else if(res == nums[i]){
                count++;
            }else if(res != nums[i]){
                count--;
            }
        }
        return res;
    }
}
```

### 解法三：分治  

如果有一个数字出现次数超过了一半，那将这个数组分成两半后，那这个数必定至少在其中一部分超过一半。  
反证：如果这个数字在两半里都没超过一半，那在整个数组里必定也没超过一半。所以可以分别求两个子数组里超过一半的数字，然后选出两个里真正的出现最多的。
- 如果这两个数字相同，那必定就是它了；   
- 如果这两个数字不同，统计两个数字分别在数组里出现的次数，大的就是了；

```
class Solution {
    public int majorityElement(int[] nums) {
        return majorityElement(nums, 0, nums.length-1); //区间是左闭右闭；
    }
    private int majorityElement(int[] nums, int left, int right){
        if(left == right){
            return nums[left];  //结束条件；
        }
        int mid = left+((right-left) >> 1);
        int leftnum = majorityElement(nums, left, mid);  //左右两侧的超过一半的数字；
        int rightnum = majorityElement(nums, mid+1, right);

        if(leftnum == rightnum) return leftnum;
        int leftcount = countNum(nums, leftnum, left, right);  //比较两个数字谁次数多；
        int rightcount = countNum(nums, rightnum, left, right);
        return leftcount > rightcount ? leftnum : rightnum;
    }
    private int countNum(int[] nums, int num, int left, int right){
        int count = 0;
        for(int i = left; i <= right; i++){
            if(nums[i] == num){
                count++;
            }
        }
        return count;
    }
}
```


