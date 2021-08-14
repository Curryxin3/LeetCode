## 137. 只出现一次的数字 II(剑指offer 56-II)
> **知识点：哈希表；位运算**
### 题目描述

给你一个整数数组 nums ，除某个元素仅出现 一次 外，其余每个元素都恰出现 三次 。请你找出并返回那个只出现了一次的元素。

你的算法应该具有线性时间复杂度。 你可以不使用额外空间来实现吗？
##### 示例

```
输入：nums = [2,2,3,2]
输出：3

输入：nums = [0,1,0,1,0,1,99]
输出：99
```
---
### 解法一：哈希表
和136题一样，使用哈希表存储每个数字出现的数字，再遍历哈希表，看谁能等于1；  
很容易想到，但这样做的坏处就是需要开辟新的空间；
```
class Solution {
    public int singleNumber(int[] nums) {
        Map<Integer,Integer> map = new HashMap<>();
        for(Integer i : nums){
            map.put(i, map.getOrDefault(i, 0)+1);
        }
        for(Integer i: map.keySet()){
            if(map.get(i) == 1) return i;
        }
        return -1;
    }
}
```
时间复杂度：O(N);  
空间复杂度：O(N);
### 解法二：位运算
这题不能再用异或去解了，因为变成了三个没有办法抵消，但是想象一下，如果有三个一模一样的数字，那这三个数字二进制相加后，所有位上要么是0；要么全是3的倍数；然后我们的多余元素，要么加上去为0；要么加上去多了一个1，所以可以依次求每位的和，然后%3，如果值为1，那证明我们在这位上的值为1；否则为0；  
如下图所示；

![image](https://note.youdao.com/yws/public/resource/806e27c4466efcf75e18b51ca94d39c9/xmlnote/8BCD946706ED4BE1AA6BE51C718A2865/7646)
```
class Solution {
    public int singleNumber(int[] nums) {
        //在java中int类型是32位，我们需要统计所有数字在某一位置的和能不能被3整除，
        // 如果不能被3整除，说明那个只出现一次的数字的二进制在那个位置是1……把32位全部统计完为止
        int ans = 0;
        for(int i = 0; i < 32; i++){
            int count = 0; //统计1的个数；
            for(int j = 0; j < nums.length; j++){
                count += (nums[j] >> i) & 1; //统计所有数在第i位上1的个数；
            }
            if(count % 3 != 0){
                ans = ans | (1 << i); //其他位不变，第i位置为1；
            }
        }
        return ans;
    }
}
```
时间复杂度：O(N);   
空间复杂度：O(1);
### 体会
**掌握位运算的解决方法：这种题目往往要按位与、按位异或等操作**；  
此外还会有左移<<;右移>>等；比如说：  
> a & 1 ： a的其他位全为0，最后一位不变：即取a最后一位；  
a | (1 << i) : a的其他位不变，把a的第i位置为1；  
(a >> i) & 1 : 取出a第i位上的值；
