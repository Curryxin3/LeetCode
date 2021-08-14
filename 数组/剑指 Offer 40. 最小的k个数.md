## 剑指 Offer 40. 最小的k个数

> **知识点：Top-K；数组；排序；分治；堆**；
### 题目描述

输入整数数组 arr ，找出其中最小的 k 个数。例如，输入4、5、1、6、2、7、3、8这8个数字，则最小的4个数字是1、2、3、4。

 

##### 示例

```
输入：arr = [3,2,1], k = 2
输出：[1,2] 或者 [2,1]

输入：arr = [0,1,2,1], k = 1
输出：[0]
```
---

### 解法一：排序     

这是一种题型：**Top-K问题** ：比如求前k大，前k小，第k大，第k小；

解决这类问题通常有三种方法：   
- 1.快速排序：直接利用排序将整个数组排好，不论是前k大，前k小，第k大，第k小都可以解决。
- 2.快速选择：快速选择就是充分利用了快速排序的思想，如果我们只是想求前k大，或者第k大，那无需对整个数组进行排序，快速排序排完一次后哨兵位就归位了，而且其前面都比哨兵位小，后面都比哨兵位大，只要
哨兵位等于k，那就找到了答案，而不用再去处理子数组了。   
- 3.堆。维持一个大根堆或者小根堆，数组中的前k大都依次入堆，最后返回堆即为答案。

可以直接将整个数组进行排序，那前k个自然就是最小的，这也是第一时间的想法；    

```
class Solution {
    public int[] getLeastNumbers(int[] arr, int k) {
        Arrays.sort(arr);
        int[] res = new int[k];
        for(int i = 0; i < k; i++){
            res[i] = arr[i];
        }
        return res;
    }
}
``` 

### 解法二：分治:快速选择 

想一下我们刚才的排序想法，排序用什么来实现呢，自然是快速排序，再回顾一下快速排序的思想：其本质上就是分治，分而治之，每次找一个哨兵，然后将比哨兵小的都移动到前面，哨兵大的都移动到后边，然后哨兵能够回到自己的位置上，接着再分别在哨兵前序列和哨兵后序列递归；  

想一下，如果我们能够找到哨兵k它所在的位置，那么前k个就是整个数组最小的k个，比如经过第一次划分后，假设哨兵到了自己的位置索引6处，那6以前的就是整个数组最小的6个。因为我们根本不需要对前6的顺序有要求，我们只要返回前6个就可以了，所以经过这一次就可以结束了，不用把整个数组都排好了。所以一共有3种情况：   
- 如果基准位j == k，则直接返回前k个就可以了；   
- 如果基准位j < k, 代表第k个数字在右子数组中递归调用右子数组；   
- 如果基准位j > k,代表第k个数字在左子数组中，递归调用左子数组；  
所以针对最大k/最小k问题，不需要用快速排序对整个数组进行排序，只要能够找到哨兵k的位置就可以了；
```
class Solution {
    public int[] getLeastNumbers(int[] arr, int k) {
        if(k >= arr.length) return arr;
        return quickSelect(arr, 0, arr.length-1,k);
    }
    private int[] quickSelect(int[] arr, int left, int right, int k){
        int i = left, j = right;
        while(i < j){
            while(i < j && arr[j] > arr[left]) j--;
            while(i < j && arr[i] <= arr[left]) i++;
            swap(arr, i, j);
        }
        swap(arr, i, left);  //交换哨兵；
        if(k > i) return quickSelect(arr, i+1, right, k);
        if(k < j) return quickSelect(arr, left, i-1, k);
        return Arrays.copyOf(arr, k);  //截取前k个长度；
    }
    private void swap(int[] arr, int i, int j){
        int temp = arr[i];
        arr[i] = arr[j];
        arr[j] = temp;
    }
}
```

### 解法三：堆

维持一个k容量的大根堆：前k个直接入堆，后面的元素如果比堆顶大则跳过，如果比堆顶小就弹出堆顶，元素入堆。

```
class Solution {
    public int[] getLeastNumbers(int[] arr, int k) {
        if(k >= arr.length){
            return arr;
        }
        int[] res = new int[k];
        if(k == 0) return res;
        PriorityQueue<Integer> queue = new PriorityQueue<>(new Comparator<>(){
            public int compare(Integer o1, Integer o2){ //注意这是包装类；不能是int；
                return o2-o1;  //默认是小根堆，重写比较器；
            }
        });
        for(int i = 0; i < k; i++){
            queue.offer(arr[i]); //前k个都入堆；
        }
        for(int i = k; i < arr.length; i++){
            if(arr[i] < queue.peek()){
                queue.poll();
                queue.offer(arr[i]);  //发现小的就入堆；大的不用管；
            }
        }
        //堆里就是最小的k个元素；
        for(int i = 0; i < k; i++){
            res[i] = queue.poll();
        }
        return res;
    }
}
```

### 体会  

这是一种题型：**Top-K问题** ：比如求前k大，前k小，第k大，第k小；

解决这类问题通常有三种方法：   
- 1.快速排序：直接利用排序将整个数组排好，不论是前k大，前k小，第k大，第k小都可以解决。
- 2.快速选择：快速选择就是充分利用了快速排序的思想，如果我们只是想求前k大，或者第k大，那无需对整个数组进行排序，快速排序排完一次后哨兵位就归位了，而且其前面都比哨兵位小，后面都比哨兵位大，只要
哨兵位等于k，那就找到了答案，而不用再去处理子数组了。   
- 3.堆。维持一个大根堆或者小根堆，数组中的前k大都依次入堆，最后返回堆即为答案。

可以直接将整个数组进行排序，那前k个自然就是最小的，这也是第一时间的想法； 

### 相关题目

[215. 数组中的第K个最大元素](https://www.cnblogs.com/Curryxin/p/15133554.html)

### 相关链接
[TopK问题](https://leetcode-cn.com/problems/zui-xiao-de-kge-shu-lcof/solution/3chong-jie-fa-miao-sha-topkkuai-pai-dui-er-cha-sou/)
