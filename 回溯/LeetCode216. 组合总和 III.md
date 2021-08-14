## 216. 组合总和 III

> **知识点：递归；回溯；组合；剪枝**
### 题目描述

找出所有相加之和为 n 的 k 个数的组合。组合中只允许含有 1 - 9 的正整数，并且每种组合中不存在重复的数字。

说明：

##### 示例

```
输入: k = 3, n = 7
输出: [[1,2,4]]

输入: k = 3, n = 9
输出: [[1,2,6], [1,3,5], [2,3,4]]
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

看一下本题和77题的区别，依然是在几个数中选k个数做组合，区别就在于本题要使最后选出来的和为某一定值。所以可以在终止条件处当选够k个数后还要判断一下和是否为目标值，是的话再将路径加入结果集。   

此外，本道题的剪枝有两处    
- 从1-9排列的，所以当某处和大于目标值后，后面的和就更大了，所以可以剪枝；   
- 无法凑够k个数的时候可以剪枝；   


![image](https://note.youdao.com/yws/public/resource/2946380d68a8807311ffcfab2987253a/xmlnote/0D28549B9F2F485FA8FFE9CA22B9985C/15021)    

```
class Solution {
    public List<List<Integer>> combinationSum3(int k, int n) {
        List<List<Integer>> res = new ArrayList<>();
        Stack<Integer> path = new Stack<>();
        backtrack(k, n, 1, 0, res, path);
        return res;
    }
    private void backtrack(int k, int n, int begin, int sum, List<List<Integer>> res, Stack<Integer> path){
        if(sum > n){
            return; //剪枝；
        }
        if(path.size() == k){
            if(sum == n){
                res.add(new ArrayList<>(path));
            }
            return;
        }
        for(int i = begin; i <= 9-(k-path.size())+1; i++){
            //做选择；
            path.push(i);
            sum += i;
            //递归：开始下一轮选择；
            backtrack(k, n, i+1, sum, res, path);
            //撤销选择：回溯；
            path.pop();
            sum -= i;
        }
    }
}
```

### 体会

- 要能够把这种决策树画出来；
- 在求和问题中，排序之后加上剪枝是很常见的操作，能够舍弃无关的操作；  

### 相关链接  

[组合总和3](https://leetcode-cn.com/problems/combination-sum-iii/solution/dai-ma-sui-xiang-lu-dai-ni-xue-tou-hui-s-petp/)
