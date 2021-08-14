## 28. 实现 strStr()
> **知识点：字符串；KMP算法**
### 题目描述

实现 strStr() 函数。

给你两个字符串 haystack 和 needle ，请你在 haystack 字符串中找出 needle 字符串出现的第一个位置（下标从 0 开始）。如果不存在，则返回  -1 。

**说明**
当 needle 是空字符串时，我们应当返回什么值呢？这是一个在面试中很好的问题。

对于本题而言，当 needle 是空字符串时我们应当返回 0 。这与 C 语言的 strstr() 以及 Java 的 indexOf() 定义相符。

##### 示例
```
输入：haystack = "hello", needle = "ll"
输出：2

输入：haystack = "aaaaa", needle = "bba"
输出：-1

输入：haystack = "", needle = ""
输出：0
```
---
### 解法一：暴力法
直接对于haystack中的每一位，依次比较needle，如果发生不匹配了，则移动到haystack中的下一位；
```
class Solution {
    public int strStr(String haystack, String needle) {
        if(needle.length() == 0) return 0;
        for(int i = 0; i <= haystack.length()-needle.length(); i++){ //注意判断条件；
            int j = 0;
            while(j < needle.length() && haystack.charAt(i+j) == needle.charAt(j)){
                j++;
            }
            if(j == needle.length()) return i; //满足证明完全匹配上了；
        }
        return -1;
    }
}
```
时间复杂度：O(NM);
### 解法二：KMP算法
可以查看[KMP算法](https://www.cnblogs.com/Curryxin/p/15014196.html)         
KMP算法的本质上就是在利用先前的信息减少不可能的匹配，把不可能的匹配都直接过滤。而这个过滤的依据就是next数组，next数组里的数的含义是以此数结尾时，最多能用前面几个数来顶替掉我们后面几个数。      
要清楚我们根据next数组去过滤是不可能漏掉情况的。为什么呢？比如说ababca，我到了最后一个a处匹配不上了，那按照上面的解法，现在应该看a处前一位也就是c的对应next值，很明显next[4]是0，所以模式串的指针就移动到最开始，也就等于将模式串的首位直接对齐到了主串中原本与a不匹配的那个位置，中间可能错过某些可能吗？比如说我最开始的ab可以移动到2,3位置处，那也是ab说不定可能呢，其实这是不可能的，为什么呢，反证法；如果0和1处的ab匹配上了2和3处的ab，那么后一个一定不匹配，因为如果匹配的话，那就是说前3个能匹配上后3个，那next[4]就等于3而不是0了，所以这也就说明了为什么我们是看前一个也就是看c的next值，因为无论怎样，我们都是要经过它的，如果不看它，那它前面几个匹配上了，到它也匹配不上；
```
class Solution {
    public int strStr(String haystack, String needle) {
        if(needle.length() == 0) return 0;
        if(haystack.length() == 0) return -1;
        char[] s = haystack.toCharArray();
        char[] p = needle.toCharArray();
        return KMP(s,p);
    }
    private int KMP(char[] s, char[] p){
        int tar = 0; //主串中待匹配的位置；
        int pos = 0; //模式串中待匹配的位置；
        int m = s.length;
        int n = p.length;
        int[] next = next(p, n); //构建next数组；
        while(tar < m){
            if(s[tar] == p[pos]){ //匹配上就接着比下一个；
                tar++;
                pos++;
            }else if(pos != 0){ //匹配不上了，而且pos指针还没移动到模式串最开始；
                pos = next[pos-1]; //看匹配不上的前一位的next数组值；
            }else{ //pos已经到模式串最开始了，而且会经过最开始的if，如果这也没匹配上，说明tar对应的值匹配不上，直接向后移一位；
                tar++;
            }
            if(pos == n){ //pos到了模式串最后，全匹配上了；
                return tar-n;
            }
        }
        return -1;
    }
    private int[] next(char[] p, int n){
        int[] next = new int[n];
        int now = 0;
        int i = 1;
        while(i < n){
            if(p[now] == p[i]){
                now++;
                next[i] = now;
                i++;
            }else if(now != 0){ //now没回到最开始呢；
                now = next[now-1]; //使得A的K前缀等于B的K后缀的最大K；A和B又一样。所以就看A就可以了；
                //看now前一位的最长前后缀；
            }else{
                next[i] = now; //移动到最开始了还不行，那就是0了；
                i++;
            }
        }
        return next;
    }
}
```
时间复杂度：O(N)+O(M);
### 体会
要掌握字符串匹配，要掌握KMP算法，要经常看这篇文章：[KMP算法](https://www.cnblogs.com/Curryxin/p/15014196.html)  
