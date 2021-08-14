## 剑指 Offer 58 - II. 左旋转字符串
> **知识点：字符串**；
### 题目描述

字符串的左旋转操作是把字符串前面的若干个字符转移到字符串的尾部。请定义一个函数实现字符串左旋转操作的功能。比如，输入字符串"abcdefg"和数字2，该函数将返回左旋转两位得到的结果"cdefgab"。


##### 示例

```
输入: s = "abcdefg", k = 2
输出: "cdefgab"

输入: s = "lrloseumgh", k = 6
输出: "umghlrlose"
```
---
### 解法一：调用API
直接调用API String的substring()方法；注意前开后闭；
```
class Solution {
    public String reverseLeftWords(String s, int n) {
        return s.substring(n,s.length())+s.substring(0,n); //前开后闭；
    }
}
```
时间复杂度：O(N);     
空间复杂度：O(N);
### 解法二：列表拼接
直接构造一个数组，对字符串进行遍历，依次填进数组；
```
class Solution {
    public String reverseLeftWords(String s, int n) {
        StringBuilder sb = new StringBuilder();
        for(int i = n; i < s.length(); i++){
            sb.append(s.charAt(i));
        }
        for(int i = 0; i < n; i++){
            sb.append(s.charAt(i));
        }
        return sb.toString();
    }
}
```
时间复杂度：O(N);       
空间复杂度：O(N);
### 解法三：字符串拼接
如果规定不能用数组，只能用字符串，可以用字符串拼接；        
注意列表拼接的空间复杂度是最高的，因为字符串在Java中是不可变量，所以每次拼接都会申请新内存。
```
class Solution {
    public String reverseLeftWords(String s, int n) {
        String str = ""; 
        for(int i = n; i < n+s.length(); i++){
            str += s.charAt(i % s.length());  //利用取余简化程序；
        }
        return str;
    }
}
```
时间复杂度：O(N);       
空间复杂度：O(N);

### 体会
注意字符串在Java语言中是不可变量，所以只要有点变化了就会申请新内存；
