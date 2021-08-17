## 796. 旋转字符串

> **知识点：字符串；KMP算法**；
### 题目描述

给定两个字符串, A 和 B。

A 的旋转操作就是将 A 最左边的字符移动到最右边。 例如, 若 A = 'abcde'，在移动一次之后结果就是'bcdea' 。如果在若干次旋转操作之后，A 能变成B，那么返回True。

##### 示例

```
示例 1:
输入: A = 'abcde', B = 'cdeab'
输出: true

示例 2:
输入: A = 'abcde', B = 'abced'
输出: false
```
---

### 解法一：暴力法

依次后移依次比较

```
class Solution {
    public boolean rotateString(String s, String goal) {
        if(s.length() != goal.length()) return false;
        if(s.length() == 0) return true;
        search:
        for(int i = 0; i < s.length(); i++){  //旋转的次数；
            for(int j = 0; j < goal.length(); j++){
                if(s.charAt((i+j) % s.length())!= goal.charAt(j)){
                    continue search;  //匹配不上直接比较下一次旋转；
                }
            }
            return true;
        }
        return false;
    }
}
```    
### 解法二：API

其实A+A里就包含了所有A进行旋转的结果，所以只要判断B是否是A的子串就可以了，而String类含有contains方法；

```
class Solution {
    public boolean rotateString(String s, String goal) {
        return (s.length() == goal.length()) && ((s+s).contains(goal));
    }
}
```
### 解法三：KMP算法   
按照上面的分析，如果把A拓展成为A+A,那这道问题就变成了在A+A中是否含有B的子串，这是经典的字符串匹配问题，最经典的当属于KMP算法。

```
class Solution {
    public boolean rotateString(String s, String goal) {
        int n = goal.length();
        int[] next = buildNext(goal, n);
        if(s.length() != goal.length()) return false;
        int i = 0;
        int now = 0;
        String news = s+s;
        while(i < news.length()){
            if(news.charAt(i) == goal.charAt(now)){
                i++;
                now++;
            }else if(now != 0){
                now = next[now-1];
            }else{
                i++;
            }
            if(now == goal.length()){
                return true;
            }
        }
        return false;
    }
    private int[] buildNext(String str, int n){
        int[] next = new int[n];
        int now = 0;
        int i = 1;
        while(i < n){
            if(str.charAt(i) == str.charAt(now)){
                now++;
                next[i] = now;
                i++;
            }else if(now != 0){
                now = next[now-1];
            }else{
                next[i] = 0;
                i++;
            }
        }
        return next;
    }
}
```

### 相关题目

[28. 实现 strStr()](https://www.cnblogs.com/Curryxin/p/15047089.html)

### 相关链接   

[KMP算法](https://www.cnblogs.com/Curryxin/p/15014196.html)


