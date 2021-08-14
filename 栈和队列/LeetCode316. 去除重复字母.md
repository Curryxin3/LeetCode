## 316. 去除重复字母
> **知识点：栈；单调**
### 题目描述

给你一个字符串 s ，请你去除字符串中重复的字母，使得每个字母只出现一次。需保证 返回结果的字典序最小（要求不能打乱其他字符的相对位置）。

##### 示例

```
输入：s = "bcabc"
输出："abc"  

输入：s = "cbacdcbc"
输出："acdb"

```
---
### 解法一：单调

我们来仔细分析一下这道题目，它结合了很多知识点，因为题目中提出了很多要求。    
1.去掉重复的；对于去重最常用的就是set，可以新建一个set往里放元素达到去重的目的；或者可以直接新建一个数组，当我们把元素放到某一容器里时，通过一个标志位来看此元素是否在容器里出现。    
2.保证顺序；这个可以用栈来实现，再结合上要求1，那就可以定义一个map，key是字符，value是true或false；只要入栈了那就true，保证后面的一样的元素不会再入栈了。  
3.返回的字典序最小；这个要求什么意思，就是说哦比如例1，我们可以返回bca，但是答案应该是abc，因为abc字典序小，这怎么实现呢，我们应该要想到单调栈，比如前面的bc比a要大，在a入栈的时候直接弹走，保证栈是单调递增的。但是这也是有错误的，比如bcab，要是按这样那答案成了ab了，c没了，所以如果a之前栈里的元素在整个数组中只出现了一次，那就不能管单调了，人家都得留下来，就这独苗了。**所以说这并不是严格意义上的单调栈。但是也用到了单调栈的思想**。

```
class Solution {
    public String removeDuplicateLetters(String s) {
        Stack<Character> stack = new Stack<>();
        int[] count = new int[256];  //记录每个字符的次数；
        for(int i = 0; i < s.length(); i++){
            count[s.charAt(i)]++;
        }
        boolean[] instack = new boolean[256];  //记录是否在栈中,用于去重；
        for(Character c : s.toCharArray()){
            count[c]--;
            if(instack[c]) continue;  //栈里有这个元素,那再来一个就不用管了；
            while(!stack.isEmpty() && c < stack.peek()){
                //维持一个单调递增栈；
                if(count[stack.peek()] == 0){
                    break;  //独苗不能弹；
                }
                instack[stack.pop()] = false; 
            }
            stack.push(c);
            instack[c] = true;
        }
        StringBuilder sb = new StringBuilder();
        while(!stack.isEmpty()){
            sb.append(stack.pop());
        }
        return sb.reverse().toString();
    }
}
```   

### 体会

这道题目很有意思，用到了很多思想，要做熟彻底弄会。

### 相关链接
[去掉重复字母](https://leetcode-cn.com/problems/remove-duplicate-letters/solution/you-qian-ru-shen-dan-diao-zhan-si-lu-qu-chu-zhong-/)
