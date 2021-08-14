## 496.下一个更大元素I
> **知识点：栈；哈希表；单调**

### 题目描述

给你两个 **没有重复元素** 的数组 nums1 和 nums2 ，其中nums1 是 nums2 的子集。  
请你找出 nums1 中每个元素在 nums2 中的下一个比其大的值。  
nums1 中数字 x 的下一个更大元素是指 x 在 nums2 中对应位置的右边的第一个比 x 大的元素。如果不存在，对应位置输出 -1 

##### 示例

```
输入: nums1 = [4,1,2], nums2 = [1,3,4,2].
输出: [-1,3,-1]
解释:
    对于 num1 中的数字 4 ，你无法在第二个数组中找到下一个更大的数字，因此输出 -1 。
    对于 num1 中的数字 1 ，第二个数组中数字1右边的下一个较大数字是 3 。
    对于 num1 中的数字 2 ，第二个数组中没有下一个更大的数字，因此输出 -1 。
```
##### 提示
- num1和num2中所有整数互不相同
### 解法一：暴力法

直接遍历数组1，然后对于数组2从后往前找。

```
class Solution {
    public int[] nextGreaterElement(int[] nums1, int[] nums2) {
        int[] maxArray = new int[nums1.length];
        int j = nums2.length;
        int max = -1;
        for(int i = 0; i < nums1.length; i++){
            while(nums2[j-1] != nums1[i]){
                if(nums2[j-1] > nums1[i]) max = nums2[j-1];
                j--;
            }
            maxArray[i] = max;
            max = -1;
            j = nums2.length;
        }
        return maxArray;
    }
}
```
时间复杂度：O(MN);

### 解法二：单调栈+哈希表
这道题目可以采用单调栈的方法，什么是单调栈呢，就是保证栈从上到下是单调的。
所以这道题目可以这样：我们先遍历nums2数组，从头开始，如果遍历到的元素大于栈顶的元素，那证明这就是栈顶元素对应的那个下一个比它更大的元素，用hashmap把它存起来，如果不是，那就也把这个元素压栈；这时候压栈的都是小于栈顶元素的，所以栈其实是单调的，直到找到比它大的，接着依次与栈顶元素比较；然后再对应nums1去在hashmap里寻找就可以了；
```
class Solution {
    public int[] nextGreaterElement(int[] nums1, int[] nums2) {
        Stack<Integer> stack = new Stack<>();
        Map<Integer,Integer> hashMap = new HashMap<>();
        for(int i = 0; i < nums2.length; i++){
            if(stack.isEmpty()) stack.push(nums2[i]);
            while(!stack.isEmpty() && nums2[i] > stack.peek()){
                hashMap.put(stack.pop(),nums2[i]);
            }
            stack.push(nums2[i]);
        }
        int[] maxArray = new int[nums1.length];
        for(int i = 0; i < nums1.length; i++){
            maxArray[i] = hashMap.getOrDefault(nums1[i], -1); //注意这个方法
        }
        return maxArray;
    }
}
```
时间复杂度：O(N+M):对于两个数组各遍历一次就可以了；
### 体会
学会这种单调栈的思想，其次要学会用hashMap；
