## 974. 和可被 K 整除的子数组
> **知识点：数组；前缀和；**
### 题目描述

给定一个整数数组 A，返回其中元素之和可被 K 整除的（连续、非空）子数组的数目。

##### 示例
```
输入：A = [4,5,0,-2,-3,1], K = 5
输出：7
解释：
有 7 个子数组满足其元素之和可被 K = 5 整除：
[4, 5, 0, -2, -3, 1], [5], [5, 0], [5, 0, -2, -3], [0], [0, -2, -3], [-2, -3]。
```
---
### 解法一：前缀和
连续子数组，又带有和-->前缀和；这和基础里的连续子数组和为k有什么区别呢，这个题元素之和可被k整除，前者我们很简单，和为k，也就是找Sm-Sn=k；现在怎么办，变成了(Sm-Sn)%k=0,那转化一下就变成了Sm%k=Sn%k；其实很好理解，如果两个数除以某个数的余数相等的话，那它们相减一定能整除k(余数相减抵消了)。     
注意题目中有一个细节：mod = (presum % k + k) % k；这样写而不是直接presum % k的原因是不同语言对带有负数的取余运算不一样；如下图； 余数满足这样的定义：a = qd + r , q 为整数，且0 ≤ |r| < |d|很明显两个都满足。这样就得到了两个余数，一个正余数r1，一个负余数r2，两者关系为：r1 = r2 + d；所以我们有了上面的操作。以后只要记住：在java、c++取余数符号跟着被除数，在python等新型语言取余数符号跟着除数。
![image](https://img-blog.csdn.net/2018091120351088?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dqMTA2Ng==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
![image](https://img-blog.csdn.net/20180911203526928?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dqMTA2Ng==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
```
class Solution {
    public int subarraysDivByK(int[] nums, int k) {
        Map<Integer,Integer> map = new HashMap<>();
        map.put(0,1);
        int count = 0;
        int presum = 0;
        for(int i = 0; i < nums.length; i++){
            presum += nums[i]; //前缀和；
            int mod = (presum % k + k) % k;  //求余数，如果两个余数相等，相减之后消去一定能整除；
            if(map.containsKey(mod)) count += map.get(mod);
            map.put(mod, map.getOrDefault(mod, 0)+1);
        }
        return count;
    }
}
```
时间复杂度：O(N);
### 体会
连续子数组+和 --> 前缀和；
