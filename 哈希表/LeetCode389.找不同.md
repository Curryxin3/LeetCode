## 389.找不同
> **知识点：哈希表。抵消思想**；

### 题目描述
给定两个字符串 s 和 t，它们只包含小写字母。
字符串 t 由字符串 s 随机重排，然后在随机位置添加一个字母。
请找出在 t 中被添加的字母。
##### 示例
```
输入；s='abcdac'; t='adaccbb';
输出；'b';
解释；因为b是被添加的元素，不一定在最后位置；
```
---
### 解法一：数组型哈希表；
这道题目可以用哈希表去做，采用数组型哈希表：**数组本身就是一个哈希表**；其索引作为一个key，其值作为value；方法如下：可以将26个字母作为key，将其出现的次数作为value，然后数组count就统计了每个字母对应在s中出现了几次；再去遍历t，每出现一个字母，就将其值减1，也就是去**抵消**，最后谁如果小于0了，那就是多出现的那个字母；
```
class Solution {
    public char findTheDifference(String s, String t) {
        int[] count = new int[26];
        for(char c : s.toCharArray()){
            count[c-'a']++;
        }
        for(char c : t.toCharArray()){
            count[c-'a']--;
            if(count[c-'a'] < 0) return c;
        }
        return ' ';
    }
}
```
### 解法二：异或；
这种思路是很巧妙的：这道题可以转换思路，将s和t合为一个字符串，然后呢，每两个字符两两按位取异或，如果两个字符一样，那取完异或就是0(字符0)。因为异或运算具有交换律，所以这样都按位异或完之后肯定有一个多余的，0和任意字符异或都等于那个字符，这样就把那个字符取出来了。**其实这也是抵消思想的一种，就是按位异或去抵消，只要相同，求完就为0；**
```
class Solution {
    public char findTheDifference(String s, String t) {
        char flag = 0;
        for(char c : (s+t).toCharArray()){
            flag ^= c;
        }
        return flag;
    }
}
```
以上两种解法都只需要将两个字符串遍历一遍就可以；
### 体会
一定要学会这种抵消的思想，很常用；    
记住两个元素按位异或后为0；0与元素A按位异或后为A；
