## 46. 全排列

> **知识点：递归；回溯；排列**
### 题目描述

给定一个不含重复数字的数组 nums ，返回其 所有可能的全排列 。你可以 按任意顺序 返回答案。

##### 示例

```
输入：nums = [1,2,3]
输出：[[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]

输入：nums = [0,1]
输出：[[0,1],[1,0]]

输入：nums = [1]
输出：[[1]]
```
---
### 解法一：回溯

回溯算法的模板：
```
result = []   //结果集
def backtrack(路径, 选择列表):
    if 满足结束条件:
        result.add(路径)  //把已经做出的选择添加到结果集；
        return  //一般的回溯函数返回值都是空；

    for 选择 in 选择列表: //其实每个题的不同很大程度上体现在选择列表上，要注意这个列表的更新，
    //比如可能是搜索起点和重点，比如可能是已经达到某个条件，比如可能已经选过了不能再选；
        做选择  //把新的选择添加到路径里；路径.add(选择)
        backtrack(路径, 选择列表) //递归；
        撤销选择  //回溯的过程；路径.remove(选择)
```

**核心就是for循环里的递归，在递归之前做选择，在递归之后撤销选择；**

---

和组合的区别就是数组可以重复使用，比如选到2的时候，可以重新选1，所以每次for的起点必须都从0开始。  
但是比如在一次树枝上，选过2，就不能再选2了，所以需要看一下路径path里是否含有，有的话就不能再选了，从栈里搜索复杂度较高，所以可以使用一个数组used来记录。 
    
```
class Solution {
    public List<List<Integer>> permute(int[] nums) {
        List<List<Integer>> res = new ArrayList<>();
        Stack<Integer> path = new Stack<>();
        boolean[] used = new boolean[nums.length];  //记录谁被选过；
        backtrack(nums, res, path, used);
        return res;
    }
    private void backtrack(int[] nums, List<List<Integer>> res, Stack<Integer> path, boolean[] used){
        if(path.size() == nums.length){
            res.add(new ArrayList<>(path));
            return;
        }
        for(int i = 0; i < nums.length; i++){
            if(used[i]) continue; //选过了就不用了
            //做选择；
            path.push(nums[i]);
            used[i] = true;
            //递归：开始下一轮选择；
            backtrack(nums, res, path, used);
            //撤销选择：回溯；
            path.pop();
            used[i] = false;
        }
    }
}
```
