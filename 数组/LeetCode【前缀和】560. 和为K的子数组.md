## 【前缀和】560. 和为K的子数组
> **知识点：数组；前缀和**；
### 题目描述

给定一个整数数组和一个整数 k，你需要找到该数组中和为 k 的连续的子数组的个数。
##### 示例
```
输入:nums = [1,1,1], k = 2
输出: 2 , [1,1] 与 [1,1] 为两种不同的情况。

```
---
### 解法一：暴力法
直接以每个元素为首开始；
```
class Solution {
    public int subarraySum(int[] nums, int k) {
        int sum = 0;
        int count = 0;
        for(int i = 0; i < nums.length; i++){  //依次以元素开始；
            for(int j = i; j < nums.length; i++){  
                sum += nums[j];
                if(sum == k) count++;
            }
            sum = 0; //一次结束后归零；
        }
        return count;
    }
}
```
时间复杂度：O(N^N);
### 解法二：前缀和
前缀和是很常见的一种解题思路，其含义就是高中学过的数列的前n项和；
![image](https://note.youdao.com/yws/public/resource/9a28193678edcdcb18a7cd937c45da85/xmlnote/9E59895E8C954EFA941F3CBEF3DC1387/7356)    
所以如果我们要求哪个子序列和为k的话，就可以转化为求前缀和数组中哪两个的值相减等于k。这熟悉吗？对啊，这不就是[两数之和](https://www.cnblogs.com/Curryxin/p/15004061.html)那个题吗？还记得怎么做的吗？就是用哈希表存，key和value分别就是数值和其索引下标，为什么存索引下标，因为那个题让我们求的就是索引下标。那类比一下，这个题呢，这个题是让我们干嘛，是求有几项和，那我们的value就是对应的前n项和为某个值的有几个，这样减出来的子序列就有几个；
```
// 前缀和没有优化；
class Solution {
    public int subarraySum(int[] nums, int k) {
        int count = 0;
        int[] premsum = new int[nums.length+1]; //比原数组长一位，第一位置为0；这样才能把nums[1]包括；
        for(int i = 0; i < nums.length; i++){ //构建前缀和数组；
            premsum[i+1] = premsum[i] + nums[i];
        } 
        for(int i = 0; i < premsum.length-1; i++){
            for(int j = i+1; j < premsum.length; j++){
                if(premsum[j] - premsum[i] == k) count++;
            }
        }
        return count;
    }
}
```
时间复杂度：依然是O(N^N);
```
// 前缀和+哈希表；
class Solution {
    public int subarraySum(int[] nums, int k) {
        Map<Integer,Integer> map = new HashMap<>();
        map.put(0,1);
        int premsum = 0;
        int count = 0;
        for(int i = 0; i < nums.length; i++){
            premsum += nums[i];  //前缀和；
            if(map.containsKey(premsum-k)) count += map.get(premsum-k);
            map.put(premsum, map.getOrDefault(premsum, 0)+1);  //前缀和为不同值的有几次；
        }
        return count;
    }
}
```
时间复杂度：O(N);
### 体会
前缀和是一种很常用的思想，要对触发它的条件敏感 **连续子数组+和**，也就是用在哪一种题型里；       
其次，哈希表也要敏感，比如在一个题目中有两数之和，那就要去用哈希表，可以利用其containsKey函数检索，从而少一次for循环；而哈希表中value的值就由我们要获得什么决定，比如此题是获得子数组的个数，那value就是每个和的次数；比如我们要获得子数组的大小或者下标，那value就是元素的索引，对症下药。
### 相关题目
[1. 两数之和](https://www.cnblogs.com/Curryxin/p/15004061.html)           
[930. 和相同的二元子数组](https://www.cnblogs.com/Curryxin/p/15022942.html)        
[724. 寻找数组的中心下标](https://www.cnblogs.com/Curryxin/p/15022592.html)      
[1248. 统计「优美子数组」](https://www.cnblogs.com/Curryxin/p/15022638.html)      
[974. 和可被 K 整除的子数组](https://www.cnblogs.com/Curryxin/p/15022745.html)   
[523. 连续的子数组和](https://www.cnblogs.com/Curryxin/p/15022897.html)         
