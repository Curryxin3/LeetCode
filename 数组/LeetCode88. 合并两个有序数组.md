## 88. 合并两个有序数组

> **知识点：数组；排序；双指针**；
### 题目描述

给你两个按 非递减顺序 排列的整数数组 nums1 和 nums2，另有两个整数 m 和 n ，分别表示 nums1 和 nums2 中的元素数目。

请你 合并 nums2 到 nums1 中，使合并后的数组同样按 非递减顺序 排列。

注意：最终，合并后数组不应由函数返回，而是存储在数组 nums1 中。为了应对这种情况，nums1 的初始长度为 m + n，其中前 m 个元素表示应合并的元素，后 n 个元素为 0 ，应忽略。nums2 的长度为 n 。

##### 示例

```
输入：nums1 = [1,2,3,0,0,0], m = 3, nums2 = [2,5,6], n = 3
输出：[1,2,2,3,5,6]
解释：需要合并 [1,2,3] 和 [2,5,6] 。
合并结果是 [1,2,2,3,5,6] ，其中斜体加粗标注的为 nums1 中的元素。   

输入：nums1 = [1], m = 1, nums2 = [], n = 0
输出：[1]
解释：需要合并 [1] 和 [] 。
合并结果是 [1] 。
```
---

### 解法一：排序  

=这道题很简单，可以直接把num2合并到num1上，也就是将两个数组合并，然后去排序；
```
class Solution {
    public void merge(int[] nums1, int m, int[] nums2, int n) {
        for(int i = m; i < m+n; i++){
            nums1[i] = nums2[i-m];
        }
        for(int i = 0; i < m+n; i++){
            for(int j = 0; j < m+n-i-1; j++){
                if(nums1[j] > nums1[j+1]){
                    int temp = nums1[j];
                    nums1[j] = nums1[j+1];
                    nums1[j+1] = temp;
                }
            }
        }
    }
}
``` 

### 解法二：双指针

上面没有用到一个条件：两个数组都是排序好的，所以可以定义两个指针，对于两个数组都从头到尾遍历。经典的以空间换时间；

```
class Solution {
    public void merge(int[] nums1, int m, int[] nums2, int n) {
        int[] arr = new int[m+n];
        int index1 = 0, index2 = 0;
        int index = 0;
        while(index1 < m || index2 < n){
            if(index1 == m){
                arr[index++] = nums2[index2++];
            }else if(index2 == n){
                arr[index++] = nums1[index1++];
            }else if(nums1[index1] <= nums2[index2]){
                arr[index++] = nums1[index1++];
            }else if(nums1[index1] > nums2[index2]){
                arr[index++] = nums2[index2++];
            }
        }
        for(int i = 0; i < m+n; i++){
            nums1[i] = arr[i];
        }
    }
}
```
