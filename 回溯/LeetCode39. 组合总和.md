## 39. 组合总和

> **知识点：递归；回溯；组合；剪枝**
### 题目描述

给定一个无重复元素的正整数数组 candidates 和一个正整数 target ，找出 candidates 中所有可以使数字和为目标数 target 的唯一组合。

candidates 中的数字可以无限制重复被选取。如果至少一个所选数字数量不同，则两种组合是唯一的。 

对于给定的输入，保证和为 target 的唯一组合数少于 150 个。

##### 示例

```
输入: candidates = [2,3,6,7], target = 7
输出: [[7],[2,2,3]]

输入: candidates = [2,3,5], target = 8
输出: [[2,2,2,2],[2,3,3],[3,5]]   

输入: candidates = [2], target = 1
输出: []  

输入: candidates = [1], target = 1
输出: [[1]]  

输入: candidates = [1], target = 2
输出: [[1,1]]
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

对于本题，有两点和77题组合不一样：    
- 此题可以重复选取选过的元素，所以选择列表的搜索起点不用i+1，仍然是i。   
- 此题没有像之前的题明确给出递归的层数，但是给了target，所以如果相加>target,那就证明到头了；   

我们换个角度重新画这个图，和77题有点差距，理解的更全面一点。 其实这就是一个**横向循环和纵向的递归**，横向循环做出不同的选择，纵向在不同的选择基础上做下一步选择。

![image](https://note.youdao.com/yws/public/resource/2946380d68a8807311ffcfab2987253a/xmlnote/049D34487C0E4F27B946B85E7F9ADC3E/14926)   

```
class Solution {
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        List<List<Integer>> res = new ArrayList<>();
        Stack<Integer> path = new Stack<>();
        backtrack(candidates, target, 0, 0, res, path);
        return res;
    }
    private void backtrack(int[] candidates, int target, int sum, int begin, List<List<Integer>> res, Stack<Integer> path){
        if(sum > target){
            return; 
        }
        if(sum == target){
            res.add(new ArrayList<>(path));
            return;
        }
        for(int i = begin; i < candidates.length; i++){
            //做选择；
            sum += candidates[i];
            path.push(candidates[i]);
            //递归：开始下一轮选择；
            backtrack(candidates, target, sum, i, res, path);  //不用+1，可以重复选；
            //撤销选择：回溯
            sum -= candidates[i];
            path.pop();
        }
    }
}
```
### 解法二:剪枝优化

上述程序有优化的空间，我们可以对数组先进行排序，然后如果找到了当前的sum已经等于target或大于target了，那后面的就可以直接跳过了，因为后面的元素更大，肯定更大于target。

![image](https://note.youdao.com/yws/public/resource/2946380d68a8807311ffcfab2987253a/xmlnote/46C794D8F1A6488DAECA3B38FF62A894/14943)

```
class Solution {
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        List<List<Integer>> res = new ArrayList<>();
        Stack<Integer> path = new Stack<>();
        Arrays.sort(candidates);  //排序
        backtrack(candidates, target, 0, 0, res, path);
        return res;
    }
    private void backtrack(int[] candidates, int target, int sum, int begin, List<List<Integer>> res, Stack<Integer> path){
        if(sum == target){
            res.add(new ArrayList<>(path));
            return;
        }
        for(int i = begin; i < candidates.length && sum + candidates[i] <= target; i++){ 
            //剪枝：如果sum+candidates[i] > target就结束；
            //做选择；
            sum += candidates[i];
            path.push(candidates[i]);
            //递归：开始下一轮选择；
            backtrack(candidates, target, sum, i, res, path);  //不用+1，可以重复选；
            //撤销选择：回溯
            sum -= candidates[i];
            path.pop();
        }
    }
}
```

### 体会

- 要能够把这种决策树画出来；
- 在求和问题中，排序之后加上剪枝是很常见的操作，能够舍弃无关的操作；  

### 相关链接  

[回溯算法入门级介绍！](https://leetcode-cn.com/problems/permutations/solution/hui-su-suan-fa-python-dai-ma-java-dai-ma-by-liweiw/)
[组合问题](https://leetcode-cn.com/problems/combinations/solution/hui-su-suan-fa-jian-zhi-python-dai-ma-java-dai-ma-/)
