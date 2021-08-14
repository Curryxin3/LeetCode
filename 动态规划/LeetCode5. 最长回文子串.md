## 5. 最长回文子串
> **知识点：动态规划；回文串**
### 题目描述

给你一个字符串 s，找到 s 中最长的回文子串。

##### 示例

```
输入：s = "babad"
输出："bab"
解释："aba" 同样是符合题意的答案。

输入：s = "cbbd"
输出："bb"

输入：s = "a"
输出："a"

输入：s = "ac"
输出："a"
```
---
### 解法一：暴力法

从头开始找最长回文子串；记录最长串的开始位置和长度，最后从s中截取就可以了；

```
class Solution {
    public String longestPalindrome(String s) {
        int len = s.length();
        if(len < 2) return s;
        int maxLen = 1;
        int begin = 0;  //得到起始位置和长度就能得到了；
        char[] ch = s.toCharArray();
        for(int i = 0; i < len-1; i++){
            for(int j = i+1; j < len; j++){
                //最长回文子串：是回文串，而且大于之前的长度；
                if(j-i+1 > maxLen && isHuiWen(ch, i, j)){
                    maxLen = j-i+1; 
                    begin = i;
                }
            }
        }
        return s.substring(begin, begin+maxLen);
    }
    private boolean isHuiWen(char[] ch, int start, int end){
        while(start < end){
            if(ch[start] == ch[end]){
                start++;
                end--;
            }else{
                return false;
            }
        }
        return true;
    }
}
```
### 解法二：动态规划；   

此题可以用动态规划去做，因为回文串天然具有状态转移的性质，比如一个子串如果是回文子串，那把前后两个字符去掉后依然是回文子串。那反过来，就可以根据前面的子串来判断新的子串了。  

- **1.确定dp数组和其下标的含义**；dp[i][j]表示子串s[i:j]是否是回文子串；
- **2.确定递推公式，即状态转移方程**；先填写j，因为i必须在j前面，i<j;判断首尾字符，如果不相等直接false，如果相等，那就状态转移，dp[i][j] = dp[i+1][j-1]; 
- **3.dp初始化**；base case; 长度为1的，也就是dp[i][i]=true；   

```
class Solution {
    public String longestPalindrome(String s) {
        int maxLen = 1;
        int begin = 0;
        int len = s.length();
        boolean[][] dp = new boolean[len][len]; //s[i:j]是否是回文串；
        for(int i = 0; i < len; i++){
            dp[i][i] = true;  //长度为1肯定都是；
        }
        char[] ch = s.toCharArray();
        for(int j = 1; j < len; j++){
            for(int i = 0; i < j; i++){
                //头尾不等，肯定不是；
                if(ch[i] != ch[j]){
                    dp[i][j] = false;
                }else{
                    //两者中间没有元素或者有一个元素，true；
                    if(j-i <= 2){
                        dp[i][j] = true;
                    }else{
                        dp[i][j] = dp[i+1][j-1]; //状态转移；
                    }
                }
                if(dp[i][j] && j-i+1 > maxLen){
                    maxLen = j-i+1;
                    begin = i;
                }
            }
        }
        return s.substring(begin,begin+maxLen);
    } 
}
```

### 解法三：中心扩散；   

从头到尾去进行遍历，然后以该元素为中心值去扩散，比较扩散的两端的值是否相等，相等的话就接着扩散，不等的话就返回最大子串的长度。    

注意细节就是**奇数和偶数是不同的**，比如aba以b扩散，和abba以bb进行扩散，所以扩散的时候左右两点分别是[i,i]和[i,i+1];   

```
class Solution {
    public String longestPalindrome(String s) {
        int len = s.length();
        int maxLen = 1;
        int begin = 0;
        for(int i = 0; i < len-1; i++){
            int len1 = expandCenter(s, i, i); //偶数中心扩散；
            int len2 = expandCenter(s, i, i+1); //奇数中心扩散；
            len1 = Math.max(len1, len2);
            if(len1 > maxLen){
                maxLen = len1;
                begin = i-(maxLen-1)/2; //奇数：i-maxLen/2; 偶数：i-maxLen/2+1; 将两者统一；
            }
        }
        return s.substring(begin, begin+maxLen);
    }
    private int expandCenter(String s, int left, int right){
        while(left >= 0 && right < s.length() && s.charAt(left) == s.charAt(right)){
            left--;
            right++;
        }
        return right-left-1;  //最后两个是已经越界的。right-left+1-2;
    }
}
```
