## 349. 两个数组的交集
> **知识点：set; 双指针**
### 题目描述

给定两个数组，编写一个函数来计算它们的交集。

##### 示例

```
输入：nums1 = [1,2,2,1], nums2 = [2,2]
输出：[2]

输入：nums1 = [4,9,5], nums2 = [9,4,9,8,4]
输出：[9,4]
```
---
### 解法一：set
交集，这不就是说的重复吗？果断想到set；遍历一个数组，把不重复的存入set，再遍历另一个数组，如果set里有，那就是一个交集，防止多算，把它移除，接着遍历。
```
class Solution {
    public int[] intersection(int[] nums1, int[] nums2) {
        List<Integer> list = new ArrayList<>();
        Set<Integer> set = new HashSet<>();
        for(Integer i : nums1){
            if(!set.contains(i)) set.add(i);
        }
        for(Integer i : nums2){
            if(set.contains(i)){
                set.remove(i);
                list.add(i);
            }
        }
        return list.stream().mapToInt(Integer::valueOf).toArray();
        //要学会这个list<Integer>转为int[];此外也可以这样
        //index = 0;
        //int[] arr = new int[list.size()];
        //for(Integer i : list){
        //   arr[index++] = i;
        //}
        //return arr;
    }
}
```
时间复杂度:O(N+M);
### 解法二：排序+双指针
先将两个数组排序，这样两个数组都是从小到大了，然后使用两个指针同时遍历这两个数组，如果两个指针指的元素相等就是交集加入list，如果两个指针指的值不等，则移动较小的元素，接着比较。直到有一个指针走到了头。**其实就是维持了一个从小到大的顺序。这样方便比较**。   
注意要比较新加入的值和已经加入的值是否相同；
```
class Solution {
    public int[] intersection(int[] nums1, int[] nums2) {
        Arrays.sort(nums1);
        Arrays.sort(nums2);
        int length1 = nums1.length, length2 = nums2.length;
        List<Integer> list = new ArrayList<>();
        int index = 0, index1 = 0, index2 = 0;
        while (index1 < length1 && index2 < length2) {
            int num1 = nums1[index1], num2 = nums2[index2];
            if (num1 == num2) {
                // 保证加入元素的唯一性
                if (list.size() == 0 || num1 != list.get(list.size() - 1)) {
                    list.add(num1);
                }
                index1++;
                index2++;
            } else if (num1 < num2) {
                index1++;
            } else {
                index2++;
            }
        }
        int[] arr = new int[list.size()];
        for(Integer i : list){
            arr[index++] = i;
        }
        return arr;
    }
}
```

### 相关题目
[350 两个数组的交集II](https://www.cnblogs.com/Curryxin/p/15054512.html)
