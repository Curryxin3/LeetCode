## 215. 数组中的第K个最大元素

> **知识点：数组；排序；分治；堆；单调**；

### 题目描述

给定整数数组 nums 和整数 k，请返回数组中第 k 个最大的元素。

请注意，你需要找的是数组排序后的第 k 个最大的元素，而不是第 k 个不同的元素。

##### 示例

```
输入: [3,2,1,5,6,4] 和 k = 2
输出: 5

输入: [3,2,3,1,2,4,5,5,6] 和 k = 4
输出: 4
```
---

### 解法一：排序     

这道题和剑指offer 40题一样，都是**Top-K问题**，所以可以使用排序，快速选择，堆来进行求解。    

```
class Solution {
    public int findKthLargest(int[] nums, int k) {
        Arrays.sort(nums);
        return nums[nums.length-k];
    }
}
``` 

### 解法二：分治：快速选择  

直接分治然后使用快速选择；

```
class Solution {
    public int findKthLargest(int[] nums, int k) {
        return quickSelect(nums, nums.length-k, 0, nums.length-1);
    }
    private int quickSelect(int[] nums, int k, int left, int right){
        int i = left, j = right;
        while(i < j){
            while(i < j && nums[j] > nums[left]) j--;
            while(i < j && nums[i] <= nums[left]) i++;
            swap(nums, i, j);
        }
        swap(nums, left, i);
        if(k < i) return quickSelect(nums, k, left, i-1);
        if(k > i) return quickSelect(nums, k, i+1, right);
        return nums[i];
    }
    private void swap(int[] nums, int i, int j){
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }
}
```

### 解法三：堆

维持一个容量为k的小根堆，如果新来的比堆顶大，入堆；新来的比堆顶小，不管。

```
class Solution {
    public int findKthLargest(int[] nums, int k) {
        PriorityQueue<Integer> queue = new PriorityQueue<>(); //维持一个容量为k的小根堆；
        for(int i = 0; i < k; i++){
            queue.offer(nums[i]); //前k个直接入堆；
        }
        for(int i = k; i < nums.length; i++){
            if(nums[i] > queue.peek()){
                queue.poll();
                queue.offer(nums[i]);
            }  //比堆顶小的不管了，大的就更新；所以最后就是k个最大的；堆顶就是第k个；
        }
        return queue.peek();
    }
}
```
### 相关链接
[TopK问题](https://leetcode-cn.com/problems/zui-xiao-de-kge-shu-lcof/solution/3chong-jie-fa-miao-sha-topkkuai-pai-dui-er-cha-sou/)
