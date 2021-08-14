## 47. 全排列 II

> **知识点：递归；回溯；排列**
### 题目描述

给定一个可包含重复数字的序列 nums ，按任意顺序 返回所有不重复的全排列。

##### 示例

```
输入：nums = [1,1,2]
输出：
[[1,1,2],
 [1,2,1],
 [2,1,1]]

输入：nums = [1,2,3]
输出：[[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]
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

和46题的区别就在于本次含有重复数字，比如例1,我们开始做出的选择是112,121，这是以第一个1作为第一选择来的；接着我们如果拿第二个1做为第一选择，那仍然是112,121，重复了。所以需要去重，**在回溯里只要涉及到去重肯定是要用排序的**，然后比较当前元素和上一元素是否相同，而且上一元素的used是否为false。因为我们需要的是**树层间的去重，而不是树枝上的去重**，也就是是横向的，不是纵向的。 比如112，这个1是可以重复选的。 
    
![image](https://note.youdao.com/yws/public/resource/2946380d68a8807311ffcfab2987253a/xmlnote/8000EF98FAFB4D26B2D2E0FA7A7C84B5/15080)

```
class Solution {
    public List<List<Integer>> permuteUnique(int[] nums) {
        List<List<Integer>> res = new ArrayList<>();
        Stack<Integer> path = new Stack<>();
        boolean[] used = new boolean[nums.length];
        Arrays.sort(nums);
        backtrack(nums, res, path, used);
        return res;
    }
    private void backtrack(int[] nums, List<List<Integer>> res, Stack<Integer> path, boolean[] used){
        if(path.size() == nums.length){
            res.add(new ArrayList(path));
            return;
        }
        for(int i = 0; i < nums.length; i++){
            if(i > 0 && nums[i] == nums[i-1] && used[i-1] == false){
                continue;   //树层不能选一样的；
            }
            if(used[i]) continue;  //同一树枝上去重；
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
