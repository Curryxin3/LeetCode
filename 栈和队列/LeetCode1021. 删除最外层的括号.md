## 1021. 删除最外层的括号
> **知识点：栈；消消乐**
### 题目描述

有效括号字符串为空 ""、"(" + A + ")" 或 A + B ，其中 A 和 B 都是有效的括号字符串，+ 代表字符串的连接。

例如，""，"()"，"(())()" 和 "(()(()))" 都是有效的括号字符串。
如果有效字符串 s 非空，且不存在将其拆分为 s = A + B 的方法，我们称其为原语（primitive），其中 A 和 B 都是非空有效括号字符串。

给出一个非空有效字符串 s，考虑将其进行原语化分解，使得：s = P_1 + P_2 + ... + P_k，其中 P_i 是有效括号字符串原语。

对 s 进行原语化分解，删除分解中每个原语字符串的最外层括号，返回 s 。

##### 示例

```
输入：s = "(()())(())"
输出："()()()"
解释：
输入字符串为 "(()())(())"，原语化分解得到 "(()())" + "(())"，
删除每个部分中的最外层括号后得到 "()()" + "()" = "()()()"。

输入：s = "(()())(())(()(()))"
输出："()()()()(())"
解释：
输入字符串为 "(()())(())(()(()))"，原语化分解得到 "(()())" + "(())" + "(()(()))"，
删除每个部分中的最外层括号后得到 "()()" + "()" + "()(())" = "()()()()(())"。

输入：s = "()()"
输出：""
解释：
输入字符串为 "()()"，原语化分解得到 "()" + "()"，
删除每个部分中的最外层括号后得到 "" + "" = ""。
```
---
### 解法一：栈
这道题目也是有效括号的升级版，因为输入都为有效的括号，关键就在于要找到最外层括号。        
- 遇到左括号就入栈，那怎么判断是不是最外层括号呢，如果入栈前栈是空的，那就是最外层；
- 遇到右括号就出栈，那怎么判断是不是最外层括号呢，如果出栈后栈是空的，那就是最外层；
```
class Solution {
    public String removeOuterParentheses(String s) {
        Stack<Character> stack = new Stack<>();
        StringBuilder sb = new StringBuilder();
        for(Character c : s.toCharArray()){
            if(stack.isEmpty() && c == '('){
                stack.push(c);
            }else if(!stack.isEmpty() && c == '('){
                stack.push(c);
                sb.append(c);
            }
            if(c == ')'){
                stack.pop();
                if(!stack.isEmpty()){
                    sb.append(c);
                }
            }
        }
        return sb.toString();
    }
}
```
### 解法二：计数器
用一个计数器来计数判断是否是最左侧括号或者是最右侧括号；     
初始计数器为0,，遇到左括号+1，遇到右括号-1；
- 如果遇到左侧括号当前计数值=1,是最外层的；
- 如果遇到右侧括号当前计数值=0,是最外层的；     

本质上和方法1是一样的，都是在干一件事：找到不是最外侧的括号将其加入数组；
```
class Solution {
    public String removeOuterParentheses(String s) {
        int count = 0;
        StringBuilder sb = new StringBuilder();
        for(Character c : s.toCharArray()){
            if(c == '(') {
                count++;
                if(count != 1) sb.append(c);
            }
            else {
                count--;
                if(count != 0) sb.append(c);
            }
        }
        return sb.toString();
    }
}
```
时间复杂度：O(N);

### 参考链接
[删除最外层的括号](https://leetcode-cn.com/problems/remove-outermost-parentheses/solution/shuang-zhi-zhen-ji-shu-fa-by-simzhou/)
