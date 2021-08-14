## 350. 两个数组的交集 II
> **知识点：哈希表; 双指针**
### 题目描述

给定两个数组，编写一个函数来计算它们的交集。

**说明：**

输出结果中每个元素出现的次数，应与元素在两个数组中出现次数的最小值一致。
我们可以不考虑输出结果的顺序。  

**进阶：**

如果给定的数组已经排好序呢？你将如何优化你的算法？
如果 nums1 的大小比 nums2 小很多，哪种方法更优？
如果 nums2 的元素存储在磁盘上，内存是有限的，并且你不能一次加载所有的元素到内存中，你该怎么办？

##### 示例

```
输入：nums1 = [1,2,2,1], nums2 = [2,2]
输出：[2,2]

输入：nums1 = [4,9,5], nums2 = [9,4,9,8,4]
输出：[4,9]
```
---
### 解法一：哈希表
这道题和349题的区别就在于这是真的交集，包括已经重复的，所以就不能用set了。又是涉及到次数的，所以我们可以用哈希表来做：统计nums1并记录其出现的次数在哈希表中，遍历nums2，如果出现了那就加入list，并且其次数要-1；如果其次数 < 0,即使还是有也不加了。
```
class Solution {
    public int[] intersect(int[] nums1, int[] nums2) {
        Map<Integer, Integer> map = new HashMap<>();
        List<Integer> list = new ArrayList<>();
        for(Integer num : nums1){
            map.put(num, map.getOrDefault(num, 0)+1);
        }
        for(Integer num : nums2){
            if (map.getOrDefault(num, 0) > 0){
                list.add(num);
                map.put(num,map.get(num)-1);
            }
        }
        int index = 0;
        int[] arr = new int[list.size()];
        for(Integer m : list){
            arr[index++] = m;
        }
        return arr;
    }
}
```
时间复杂度:O(N+M);
### 解法二：排序+双指针
和349题一样，只不过不用判断了，只要两个一样就加入。先将两个数组排序，这样两个数组都是从小到大了，然后使用两个指针同时遍历这两个数组，如果两个指针指的元素相等就是交集加入list，如果两个指针指的值不等，则移动较小的元素，接着比较。直到有一个指针走到了头。**其实就是维持了一个从小到大的顺序。这样方便比较**。   
```
class Solution {
    public int[] intersect(int[] nums1, int[] nums2) {
        int index1 = 0, index2 = 0;
        int index = 0;
        Arrays.sort(nums1);
        Arrays.sort(nums2);
        List<Integer> list = new ArrayList<>();
        while (index1 < nums1.length && index2 < nums2.length){
            if (nums1[index1] == nums2[index2]){
                list.add(nums1[index1]);
                index1++;
                index2++;
            }else if(nums1[index1] < nums2[index2]){
                index1++;
            }else{
                index2++;
            }
        }
        int[] arr = new int[list.size()];
        for(Integer m : list){
            arr[index++] = m;
        }
        return arr;
    }
}
```
