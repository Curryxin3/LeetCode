## 1207. 独一无二的出现次数
> **知识点：集合；哈希表**
### 题目描述

给你一个整数数组 arr，请你帮忙统计数组中每个数的出现次数。

如果每个数的出现次数都是独一无二的，就返回 true；否则返回 false。。

##### 示例

```
输入：arr = [1,2,2,1,1,3]
输出：true
解释：在该数组中，1 出现了 3 次，2 出现了 2 次，3 只出现了 1 次。没有两个数的出现次数相同。

输入：arr = [1,2]
输出：false

输入：arr = [-3,0,1,-3,1,1,1,-3,10,0]
输出：true
```
---
### 解法一：哈希表+set
出现次数 --> 哈希表；  
独一无二 --> set;
```
class Solution {
    public boolean uniqueOccurrences(int[] arr) {
        Map<Integer, Integer> map = new HashMap<>();
        for(Integer i : arr){
            map.put(i, map.getOrDefault(i, 0)+1);
        }
        Set<Integer> set = new HashSet<>();
        for(Integer i : map.keySet()){
            if(set.contains(map.get(i))) return false;
            else set.add(map.get(i));
        }
        return true;
    }
}
```
时间复杂度
### 体会
出现什么统计字符或者数字出现的次数，用哈希表；   
出现什么唯一，独一无二，有无重复，用set的不可重复性；  
**统计次数 --> 哈希表；  
独一无二 --> set;**
