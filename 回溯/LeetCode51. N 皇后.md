## 51. N 皇后

> **知识点：递归；回溯；N皇后；剪枝**
### 题目描述

给定一个可包含重复数字的序列 nums ，按任意顺序 返回所有不重复的全排列。

##### 示例

![image](https://note.youdao.com/yws/public/resource/2946380d68a8807311ffcfab2987253a/xmlnote/89EE2506E8684005AD21BAF1E3746B97/15097)

```
输入：n = 4
输出：[[".Q..","...Q","Q...","..Q."],["..Q.","Q...","...Q",".Q.."]]
解释：如上图所示，4 皇后问题存在两个不同的解法。

输入：n = 1
输出：[["Q"]]
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

这是棋盘类问题，本质上还是在做选择，在构建一个决策树，只要是决策树的，那就必然是回溯算法。     

直接套模板就可以了，那不同的是什么呢，其实大部分回溯类的题不同的就是选择列表的更新，在我们这道题目里选择列表的更新受3个条件影响。不同行不同列不能对角线。  

- 终止条件：当到达最后一行，也就是row=n的时候，就可以收割结果了；   
- 做选择：我们遍历到一个列的时候，也就是for，如果path在这个点所在的行、列、对角线都没有元素那就可以放了。
    
![image](https://note.youdao.com/yws/public/resource/2946380d68a8807311ffcfab2987253a/xmlnote/F208176EA22F410E98D42500C508A210/15106)

```
class Solution {
    public List<List<String>> solveNQueens(int n) {
        List<List<String>> res = new ArrayList<>();
        char[][] path = new char[n][n];  //路径是二维数组；
        for(char[] c : path){
            Arrays.fill(c, '.');  //全部初始化为.  
        }
        backtrack(n, 0, res, path);
        return res;
    }
    private void backtrack(int n, int row, List<List<String>> res, char[][] path){
        if(row == n){
            res.add(toList(n, path));  
            return;
        }
        for(int col = 0; col < n; col++){
            if(isValid(path, row, col, n)){ //决策树的深度就是row，宽度for就是列；row和col确定唯一的点；
                path[row][col] = 'Q';  //做选择；
                backtrack(n, row+1, res, path);  //递归：下一轮选择；
                path[row][col] = '.';
            }
        }
    }
    //检测(row, col)能否放皇后；
    private boolean isValid(char[][] path, int row, int col, int n){
        //检查列；
        for(int i = 0; i < row; i++){
            if(path[i][col] == 'Q'){
                return false;
            }
        }
        //检查45度；
        for(int i = row-1, j = col-1; i >= 0 && j >= 0; i--, j--){
            if(path[i][j] == 'Q'){
                return false;
            }
        }
        //检查135度；
        for(int i = row-1, j = col+1; i >=0 && j < n; i--,j++){
            if(path[i][j] == 'Q'){
                return false;
            }
        }
        return true;
    }
    //二维数组转为list；
    private List<String> toList(int n, char[][] path){
        List<String> list = new ArrayList<>();
        for(int i = 0; i < n; i++){
            list.add(String.valueOf(path[i]));  //字符数组转为字符串；
        }
        return list;
    }
}
```
### 相关链接  

[N皇后](https://leetcode-cn.com/problems/n-queens/solution/dai-ma-sui-xiang-lu-51-n-queenshui-su-fa-2k32/)
