## 40. 组合总和 II

> **知识点：递归；回溯；组合；剪枝**
### 题目描述

给定一个数组 candidates 和一个目标数 target ，找出 candidates 中所有可以使数字和为 target 的组合。

candidates 中的每个数字在每个组合中只能使用一次。

注意：解集不能包含重复的组合。

##### 示例

```
输入: candidates = [10,1,2,7,6,1,5], target = 8,
输出:
[
[1,1,6],
[1,2,5],
[1,7],
[2,6]
]

输入: candidates = [2,5,2,1,2], target = 5,
输出:
[
[1,2,2],
[5]
]
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

本题又和39题不一样了，我们来看一下不一样的点在哪里    
- 本题数组中的每个数字在每个组合里只能选一次，不能重复选择。 -- > 所以每次的选择列表里的搜索起点从i+1开始。   
- 本题中的数组是有重复的，39题是无重复的。这带来一个问题就是可能会选到一样的，举个例子，就比如上面的例2，排完序后是12225，然后第一种组合选了122(注意这个树枝上我们是可以选第2个2的，因为本身它就有很多嘛)，然后第2个2撤销了，选第3个2，这其实是不可以的，因为这也是122，重复了。所以可以总结：**一条树枝可以重复，同一树层不可以重复**。   

所以使用一个used数组来标记那个元素被选择过。candidates[i] == candidates[i - 1] 并且 used[i - 1] == false，就说明：前一个树枝，使用了candidates[i - 1]，也就是说同一树层使用过candidates[i - 1]。


![image](https://note.youdao.com/yws/public/resource/2946380d68a8807311ffcfab2987253a/xmlnote/24C3FEE1E559439289C9A76050C95C2E/14990)    

可以看出在candidates[i] == candidates[i - 1]相同的情况下：

- used[i - 1] == true，说明同一**树支**candidates[i - 1]使用过
- used[i - 1] == false，说明同一**树层**candidates[i - 1]使用过
。

```
class Solution {
    public List<List<Integer>> combinationSum2(int[] candidates, int target) {
        List<List<Integer>> res = new ArrayList<>();
        Stack<Integer> path = new Stack<>();
        boolean[] used = new boolean[candidates.length];  //标记谁被选择过；
        Arrays.sort(candidates);
        backtrack(candidates, target, 0, 0, res, path, used);
        return res;
    }
    private void backtrack(int[] candidates, int target, int begin, int sum, List<List<Integer>> res, Stack<Integer> path, boolean[] used){
        if(sum == target){
            res.add(new ArrayList<>(path));
            return;
        }
        for(int i = begin; i < candidates.length && sum + candidates[i] <= target; i++){
            if(i > 0 && candidates[i] == candidates[i-1] && !used[i-1]) {
                continue;  //前一树枝用过了；
            } 
            //做选择
            sum += candidates[i];
            path.push(candidates[i]);
            used[i] = true;
            //递归：开始下一轮选择；
            backtrack(candidates, target, i+1, sum, res, path, used);
            //撤销选择：回溯；
            sum -= candidates[i];
            path.pop();
            used[i] = false;
        }
    }
}
```

### 体会

- 要能够把这种决策树画出来；
- 在求和问题中，排序之后加上剪枝是很常见的操作，能够舍弃无关的操作；  

### 相关链接  

[组合总和2](https://leetcode-cn.com/problems/combination-sum-ii/solution/dai-ma-sui-xiang-lu-dai-ni-xue-tou-hui-s-ig29/)
