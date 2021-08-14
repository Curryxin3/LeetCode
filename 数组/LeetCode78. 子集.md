## 78. 子集

> **知识点：数组；位运算；**；
### 题目描述

给你一个整数数组 nums ，数组中的元素 互不相同 。返回该数组所有可能的子集（幂集）。

解集 不能 包含重复的子集。你可以按 任意顺序 返回解集。

##### 示例

```
输入：nums = [1,2,3]
输出：[[],[1],[2],[1,2],[3],[1,3],[2,3],[1,2,3]]

输入：nums = [0]
输出：[[],[0]]
```
---

### 解法一：迭代     

这道题目可以用迭代的方式去解，首先在答案中添加空集，然后每遍历一个元素，就是把答案中已有的集合在，末尾追加元素。比如例1，遍历1的时候，在空集后追加1.遍历2的时候在答案中的集合空集和1后追加2，形成2和1,2；依次类推；    

```
class Solution {
    public List<List<Integer>> subsets(int[] nums) {
        List<List<Integer>> res = new ArrayList<>();
        List<Integer> list = new ArrayList<>();
        res.add(list);
        for(int num : nums){
            for(int i = 0; i < res.size(); i++){
                List<Integer> temp = new ArrayList<>(res.get(i));
                temp.add(num);  //在每个集合后添加新元素；
                res.add(temp);
            }
        }
        return res;
    }
}
``` 

### 解法二：位运算

有3个元素，那最后我们得到的子集个数一定是8个，也就是2^3个，想一下我们其实可以用3位的0,1二进制表示，哪一位上为1就对应哪一位上被选中，比如001，代表3,011代表2,3，000代表空集；这样就把所有的子集都找到了。

```
class Solution {
    public List<List<Integer>> subsets(int[] nums) {
        int n = nums.length;
        int len = 1 << n;  //一共有多少个子集；
        List<List<Integer>> res = new ArrayList<>();
        for(int i = 0; i < len; i++){
            List<Integer> list = new ArrayList<>();
            for(int j = 0; j < n; j++){
                if(((i >> j) & 1) == 0){
                    list.add(nums[j]);  //哪位上为1选中哪位；
                }
            }
            res.add(list);
        }
        return res;
    }
}
```
