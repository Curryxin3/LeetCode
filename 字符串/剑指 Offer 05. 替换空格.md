## 剑指 Offer 05. 替换空格
> **知识点：字符串**；
### 题目描述

请实现一个函数，把字符串 s 中的每个空格替换成"%20"。

##### 示例

```
输入：s = "We are happy."
输出："We%20are%20happy."
```
---
### 解法一：
因为我们不知道最后替代后的长度是多少，所以可以用可变长的数组StringBuilder或StringBuffer来，遇到空格就添加新的，其他不动。或者也可以直接创建一个长度是原数组3倍的新数组，适应一个变量来记录长度，最后读取到那里就可以了；
```
class Solution {
    public String replaceSpace(String s) {
        StringBuilder sb = new StringBuilder();
        for(char c : s.toCharArray()){
            if(c != ' '){
                sb.append(c);
            }else{
                sb.append("%20");
            }
        }
        return sb.toString();
    }
}
```
```
class Solution {
    public String replaceSpace(String s) {
        int length = s.length();
        char[] array = new char[length * 3];
        int size = 0;
        for (int i = 0; i < length; i++) {
            char c = s.charAt(i);
            if (c == ' ') {
                array[size++] = '%';
                array[size++] = '2';
                array[size++] = '0';
            } else {
                array[size++] = c;
            }
        }
        String newStr = new String(array, 0, size); //注意这个方法；
        return newStr;
    }
}
```
时间复杂度：O(N);     
空间复杂度；O(N);
