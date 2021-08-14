## 1047. 删除字符串中的所有相邻重复项
> **知识点：栈**
### 题目描述

给出由小写字母组成的字符串 S，重复项删除操作会选择两个相邻且相同的字母，并删除它们。

在 S 上反复执行重复项删除操作，直到无法继续删除。

在完成所有重复项删除操作后返回最终的字符串。答案保证唯一。

##### 示例

```
输入："abbaca"
输出："ca"
解释：
例如，在 "abbaca" 中，我们可以删除 "bb" 由于两字母相邻且相同，这是此时唯一可以执行删除操作的重复项。之后我们得到字符串 "aaca"，其中又只有 "aa" 可以执行重复项删除操作，所以最后的字符串为 "ca"。

```
---
### 解法一：自带栈
这个题应该能够发现这就是LeetCode 20 有效的括号的翻版，是要用栈来解的，逐步消消乐；   
可以直接使用自带的栈，但是别忘了最后要出栈的时候是要翻转一下的，因为顺序使反的。
```
class Solution {
    public String removeDuplicates(String s) {
        Stack<Character> stack = new Stack<>();
        for(Character c : s.toCharArray()){
            if(stack.isEmpty()) stack.push(c);
            else if(c == stack.peek()) stack.pop();
            else stack.push(c);
        }
        StringBuilder sb = new StringBuilder();
        while(!stack.isEmpty()){
            sb.append(stack.pop());
        }
        return sb.reverse().toString();
    }
}
```
### 解法二：数组栈
使用数组来模拟一个栈，这样在最后的时候不用翻转数组；
```
class Solution {
    public String removeDuplicates(String s) {
        StringBuilder sb = new StringBuilder();
        for(Character c : s.toCharArray()){
            if(sb.length() == 0) sb.append(c);
            else if(sb.charAt(sb.length()-1) == c) sb.deleteCharAt(sb.length()-1);
            else sb.append(c);
        }
        return sb.toString();
    }
}
```
### 参考链接
[删除字符中所有相邻重复项](https://leetcode-cn.com/problems/remove-all-adjacent-duplicates-in-string/solution/shan-chu-zi-fu-chuan-zhong-de-suo-you-xi-4ohr/)
