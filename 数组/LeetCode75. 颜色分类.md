## 75. 颜色分类
> **知识点：数组；双指针；**；
### 题目描述
找出数组中重复的数字。

给定一个包含红色、白色和蓝色，一共n个元素的数组，**原地**对它们进行排序，使得相同颜色的元素相邻，并按照红色、白色、蓝色顺序排列。

此题中，我们使用整数 0、 1 和 2 分别表示红色、白色和蓝色。
##### 示例
```
输入：nums = [2,0,2,1,1,0]
输出：[0,0,1,1,2,2]

输入：nums = [2,0,1]
输出：[0,1,2]

输入：nums = [0]
输出：[0]

输入：nums = [1]
输出：[1]

```
---
### 解法一：排序法
此题就是让从小到大排序，可以直接使用冒泡排序，
```
class Solution {
    public void sortColors(int[] nums) {
        for(int i = 0; i < nums.length; i++){
            for(int j = 0; j < nums.length-1-i; j++){
                if(nums[j] > nums[j+1]){
                    int temp = nums[j];
                    nums[j] = nums[j+1];
                    nums[j+1] = temp;
                }
            }
        }
    }
}
```
时间复杂度：O(N^N);
### 解法二：单指针
使用一个指针，遍历两次，先把0排好，再把1排好就行了；
```
class Solution {
    public void sortColors(int[] nums) {
        int head = 0;   //Z指示排好的序列的下一个元素即要排的序列；
        for(int i = 0; i < nums.length; i++){ //找0的过程；找到了和head的元素交换；
            if(nums[i] == 0){    
                nums[i] = nums[head];
                nums[head] = 0;
                head++;
            }
        }
        for(int i = head; i < nums.length; i++){
            if(nums[i] == 1){
                nums[i] = nums[head];
                nums[head] = 1;
                head++;
            }
        }
    }
}
```
时间复杂度：O(N)+O(N);
### 解法三：双指针
首尾各定义一个指针；首指针排0，尾指针排2，这样遍历一遍就可以了；
注意在首指针前面所有的0都归位，在尾指针后面所有的2叶归位；
```
class Solution {
    public void sortColors(int[] nums) {
        int head = 0, tail = nums.length-1;
        int i = 0;
        while(i <= tail){  //到尾指针就可以了，尾指针后面都已经搞定了；
            if(i < head) i = head;   //可能出现情况left直接超过i了，i前面都搞定了，所以可以让其从head开始；
            else if(nums[i] == 0){
                swap(nums, i, head);
                head++;
            }else if(nums[i] == 2){
                swap(nums, i, tail);
                tail--;
            }else{
                i++;
            }
        }
    }
    private void swap(int[] nums, int i, int j){
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }
}
```
时间复杂度：O(N);
