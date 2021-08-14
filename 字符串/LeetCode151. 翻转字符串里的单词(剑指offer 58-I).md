## 151. 翻转字符串里的单词(剑指offer 58-I)
> **知识点：字符串；双指针**
### 题目描述

给你一个字符串 s ，逐个翻转字符串中的所有 单词 。

单词 是由非空格字符组成的字符串。s 中使用至少一个空格将字符串中的 单词 分隔开。

请你返回一个翻转 s 中单词顺序并用单个空格相连的字符串。

说明：

输入字符串 s 可以在前面、后面或者单词间包含多余的空格。
翻转后单词间应当仅用一个空格分隔。
翻转后的字符串中不应包含额外的空格



##### 示例

```
输入：s = "the sky is blue"
输出："blue is sky the"

输入：s = "  hello world  "
输出："world hello"
解释：输入字符串可以在前面或者后面包含多余的空格，但是翻转后的字符不能包括。
```
---
### 解法一：调用API
要对字符串，包括集合里的常用方法很熟悉，这样就可以直接调用里面的常用方法，比如说去掉首尾空格，比如说以某个字符分割，比方说以某个字符连接，比方说反转；
```
class Solution {
    public String reverseWords(String s) {
        //利用API;
        s = s.trim();
        String[] str = s.split("\\s+"); //正则表达式，根据一个或多个空格分割；
        List<String> list = new ArrayList<>();
        for(String i : str){
            list.add(i); //将字符串加入列表；
        }
        Collections.reverse(list); //直接调用反转方法；
        return String.join(" ", list); //字符串常用方法，以空格连接，返回一个字符串；
    }
}
```
时间复杂度：O(N);      
空间复杂度：O(N);

### 解法二：手写方法
我们上面主要使用了两个方法：去掉空格，反转；所以我们需要手动实现这两个方法；       
1. 去掉首尾两边和中间的多余空格；      
2. 将整个字符串反转；
3. 将字符串里单个字符再反转；      
需要注意的是，在java里字符串是不可变量，所以需要将其转化为可变的数据结构，比如数组；
```
class Solution {
    public String reverseWords(String s) {
        StringBuilder sb = trimSpaces(s); 
        reverse(sb, 0, sb.length()-1);
        reverseWord(sb);
        return sb.toString();
    }
    //去掉首尾和多余空格；
    private StringBuilder trimSpaces(String s){
        int left = 0, right = s.length()-1;
        while(left <= right && s.charAt(left) == ' '){
            left++;
        } //去掉左侧空格，定位到左侧第一个无空格处；
        while(left <= right && s.charAt(right) == ' '){
            right--;
        } //同理去掉右侧空格；
        StringBuilder sb = new StringBuilder();
        while(left <= right){
            char c = s.charAt(left);
            if(c != ' ') sb.append(c);
            //到这证明遇到空格了，这时候看sb里最后一个不是空格就给加进去；
            else if(sb.charAt(sb.length()-1) != ' ') sb.append(c);
            left++;
        }
        return sb;
    }
    //反转整个字符；
    private void reverse(StringBuilder sb, int front, int tail){
        while(front < tail){
            char temp = sb.charAt(front); //注意StringBuilder底层是数组，
            //但是没有sb[front];获取指定索引就用charAt，包括String也是；
            sb.setCharAt(front, sb.charAt(tail));
            sb.setCharAt(tail, temp);
            front++;
            tail--;
        }
    }
    //反转单个单词；
    private void reverseWord(StringBuilder sb){
        int start = 0, end = 0; //end记录每个单词的最后；
        int l = sb.length();
        while(start < l){
            while(end < l && sb.charAt(end) != ' '){
                end++;
            } //end指定空格处；
            reverse(sb, start, end-1);
            start = end+1; //更新为下一个单词首处；
            end++;
        }
    }
}
```
时间复杂度：O(N);      
空间复杂度：O(N);

### 解法三：双指针
先把首尾空格去掉，然后定义两个指着，从后往前，分别指向每个单词的首和尾，再加入StringBuilder里；
```
class Solution {
    public String reverseWords(String s) {
        s = s.trim(); //去掉首尾空格；
        StringBuilder sb = new StringBuilder();
        int i = s.length()-1, j = i; //两个指着一个指向单词开头，一个指向结尾；
        while(i >= 0){
            while (i >= 0 && s.charAt(i) !=' ') i--; //i停在空格上；
            sb.append(s.substring(i+1, j+1)+' '); //前开后闭；添加单词；
            while (i >= 0 && s.charAt(i) == ' ') i--; //跳过空格；
            j = i; //j指向下一个单词的尾；
        }
        return sb.toString().trim(); //因为上面在每个单词后面都加了空格，所以把最后的结尾的删除；
    }
}
```
时间复杂度：O(N);     
空间复杂度：O(N);
### 体会
- 要知道字符串的特点：在java语言中是不可变性。所以只要是需要修改字符串就需要引入新的可变的数据结构，常用StringBuilder。
- 要对String、StringBuilder常用方法熟悉；能想到。

### 相关链接
[翻转字符串里单词](https://leetcode-cn.com/problems/reverse-words-in-a-string/solution/fan-zhuan-zi-fu-chuan-li-de-dan-ci-by-leetcode-sol/)      
[剑指offer58-I 翻转单词顺序](https://leetcode-cn.com/problems/fan-zhuan-dan-ci-shun-xu-lcof/solution/mian-shi-ti-58-i-fan-zhuan-dan-ci-shun-xu-shuang-z/)
