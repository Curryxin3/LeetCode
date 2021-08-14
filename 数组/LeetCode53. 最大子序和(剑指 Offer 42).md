## 53. 最大子序和(剑指 Offer 42)

> **知识点：数组；前缀和；哨兵；动态规划；分治**；
### 题目描述

输入一个整型数组，数组中的一个或连续多个整数组成一个子数组。求所有子数组的和的最大值。

要求时间复杂度为O(n)。

##### 示例

```
输入: nums = [-2,1,-3,4,-1,2,1,-5,4]
输出: 6
解释: 连续子数组 [4,-1,2,1] 的和最大，为 6。
```
---

### 解法一：前缀和+哨兵

连续子数组 --> 前缀和     
从前往后遍历求前缀和，维持两个变量，一个是最大子数组和，也就是答案，一个是最小的前缀和，我们可以把这个值理解为哨兵，这个就是我们用来获取答案的，因为每次前缀和-这个最小的肯定就是最大的。

```
class Solution {
    public int maxSubArray(int[] nums) {
        int pre = 0; //前缀和；
        int minPre = 0; //最小的前缀和：哨兵；
        int maxSum = Integer.MIN_VALUE;
        for(int i = 0; i < nums.length; i++){
            pre += nums[i];
            maxSum = Math.max(maxSum, pre-minPre);
            minPre = Math.min(pre, minPre);
        }
        return maxSum;
    }
}
``` 

### 解法二：贪心    

这道题贪心怎么解？贪什么呢？想一下在这个过程中，比如-2 1，我们需要-2吗？不需要！因为负数只会拉低我们最后的和，只起副作用的索性不如不要了。直接从1开始就行了； **贪的就是负数和一定会拉低结果。**   
所以我们的贪心选择策略就是：只选择和>0的，对于和<=0的都可以舍弃了。

```
class Solution {
    public int maxSubArray(int[] nums) {
        int maxSum = Integer.MIN_VALUE;
        int sum = 0;
        for(int i = 0; i < nums.length; i++){
            sum += nums[i];
            maxSum = Math.max(sum, maxSum);
            if(sum <= 0){
                sum = 0; //对于<=0的前缀和，已经没要意义了，从下一位置开始；
                continue;
            }
        }
        return maxSum;
    }
}
```   

### 解法三：分治    

这道题可以用分治去解。期望去求解一个区间[l,r]内的最大子序和，按照分而治之的思想，可以将其分为左区间和右区间。    
左区间L：[l, mid]和右区间R：[mid + 1, r].    
lSum 表示 [l,r] 内以 l 为左端点的最大子段和      
rSum 表示 [l,r] 内以 r 为右端点的最大子段和     
mSum 表示 [l,r] 内的最大子段和     
iSum 表示 [l,r] 的区间和     
递归地求解出L.mSum以及R.mSum之后求解M.mSum。因此首先在分治的递归过程中需要维护区间最大连续子列和mSum这个信息。
接下来分析如何维护M.mSum。具体来说有3种可能：

- M上的最大连续子列和序列完全在L中，即M.mSum = L.mSum
- M上的最大连续子列和序列完全在R中，即M.mSum = R.mSum
- M上的最大连续子列和序列横跨L和R，则该序列一定是从L中的某一位置开始延续到mid（L的右边界），然后从mid + 1（R的左边界）开始延续到R中的某一位置。因此我们还需要维护区间左边界开始的最大连续子列和leftSum以及区间右边界结束的最大连续子列和rightSum信息

```
class Solution {
    public class Status{
        public int lSum, rSum, mSum, iSum;
        // lSum 表示 [l,r] 内以 l 为左端点的最大子段和
        // rSum 表示 [l,r] 内以 r 为右端点的最大子段和
        // mSum 表示 [l,r] 内的最大子段和
        // iSum 表示 [l,r] 的区间和
        public Status(int lSum, int rSum, int mSum, int iSum){
            this.lSum = lSum;
            this.rSum = rSum;
            this.mSum = mSum;
            this.iSum = iSum;
        }
    }
    public Status getInfo(int[] a, int l, int r){
        if(l == r) return new Status(a[l], a[l], a[l], a[l]); //终止条件；
        int mid = l + ((r-l) >> 1);
        Status lsub = getInfo(a, l, mid);
        Status rsub = getInfo(a, mid+1, r);
        return pushUp(lsub, rsub); 
    }
    //根据两个子串得到整个序列结果；
    public Status pushUp(Status l, Status r){
        int iSum = l.iSum + r.iSum;
        int lSum = Math.max(l.lSum, l.iSum+r.lSum);
        int rSum = Math.max(r.rSum, r.iSum+l.rSum);
        int mSum = Math.max(Math.max(l.mSum, r.mSum), l.rSum+r.lSum);
        return new Status(lSum, rSum, mSum, iSum);
    }

    public int maxSubArray(int[] nums) {
        return getInfo(nums, 0, nums.length-1).mSum;
    }
}
```


### 解法四：动态规划  

- **1.确定dp数组和其下标的含义**：dp[i]表示以i结尾的连续子数组的最大和；
- **2.确定递推公式，即状态转移方程**：以i结尾想一下我们有几种可能，一种是i-1过来的，也就是上一个的连续子数组延续到i处了，那和就为dp[i-1]+nums[i],另一种呢，就是自己开始，前面那个连续子数组不行，那就是nums[i]了，想一下为什么前面那个不行，还不是前面的和会拖累自己，那就意味着前面的和是负数；这其实就引出贪心的方法了。不过我们这里不用这么麻烦，直接用一个max函数，取两者大的那个就行；
- **3.dp初始化base case**：dp[0]只有一个数，所以dp[0] = nums[0];

```
class Solution {
    public int maxSubArray(int[] nums) {
        int len = nums.length;
        int[] dp = new int[len]; //以i结尾的连续子数组的最大和为dp[i];
        if(nums == null || len <= 1) return nums[0];
        dp[0] = nums[0];
        for(int i = 1; i < len; i++){
            dp[i] = Math.max(dp[i-1]+nums[i], nums[i]); //状态转移；
        }
        //注意我们要遍历一遍返回最大的dp；
        int maxSum = dp[0];
        for(int i = 1; i < len; i++){
            maxSum = Math.max(maxSum, dp[i]);
        }
        return maxSum;
    }
}
```
当然上述程序可以优化，因为我们的dp[i]其实只和前一状态i-1有关，所以可以采用一个滚动变量来记录，而不用整个数组。 
```
class Solution {
    public int maxSubArray(int[] nums) {
        int pre = 0;  //记录前一状态；
        int res = nums[0]; //记录最后结果的最大值；
        for (int num : nums) {
            pre = Math.max(pre + num, num);
            res = Math.max(res, pre);
        }
        return res;
    }
}
```   

### 体会

这道题目是一道很典型的题目，用到了各种方法和思想。要常看常做，分治是其中比较困难的，但是要会这种思想。这道题目最好的方法还是哨兵和动态规划， 其实贪心就是从动态规划的一个特殊情况过去的，体会两者的关系；
