## 20.有效的括号
> **知识点：栈；消消乐；**
### 题目描述
给定一个只包括 '('，')'，'{'，'}'，'['，']' 的字符串 s ，判断字符串是否有效。  
有效字符串需满足：  
- 左括号必须用相同类型的右括号闭合。  
- 左括号必须以正确的顺序闭合。
##### 示例
```
输入：s = "()"
输出：true

输入：s = "()[]{}"
输出：true

输入：s = "(]"
输出：false

输入：s = "([)]"
输出：false

输入：s = "{[]}"
输出：true
```
---
### 解法一：栈
这道题目其实是典型的一道栈的题目。可以每次只要遇到左括号就将其压栈，遇到右括号就让其与栈顶元素去做对比，如果和栈顶元素配套的话就出栈，然后判断最后栈里是否还有元素；
```
class Solution {
    public boolean isValid(String s) {
        Stack<Character> stack = new Stack<>();
        for(Character c: s.toCharArray()){
            if(c == '(' || c == '{' || c == '[') stack.push(c);
            else if(stack.size() == 0) return false; //证明第一个元素为右括号，直接false；
            else if(c == ')' && stack.peek() != '(') return false;
            else if(c == '}' && stack.peek() != '{') return false;
            else if(c == ']' && stack.peek() != '[') return false;
            else stack.pop();
        }
        return stack.isEmpty();
    }
}
```

### 体会   

熟悉栈的操作，要注意与队列区分，包括创建的时候，栈是直接有栈类Stack，而队列是new的LinkedList；其次还有压栈和入队，出栈和出队；        

**栈往往会用来解决那种两两匹配，两两抵消消消乐的问题**，比如常见的有效括号等，就是判断后入栈的相邻元素是否有相互关系能够相互抵消，

### 相关题目
[1047 删除字符串中所有相邻元素](https://www.cnblogs.com/Curryxin/p/15063794.html)         
[1021 删除最外层的括号](https://www.cnblogs.com/Curryxin/p/15063856.html) 
