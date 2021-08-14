## 242. 有效的字母异位词
> **知识点：字符串；哈希表**
### 题目描述
给定两个字符串 s 和 t ，编写一个函数来判断 t 是否是 s 的字母异位词。

注意：若 s 和 t 中每个字符出现的次数都相同，则称 s 和 t 互为字母异位词。

##### 示例

```
输入: s = "anagram", t = "nagaram"
输出: true

输入: s = "rat", t = "car"
输出: false
```
---
### 解法一：排序
直接将两个字符串转化为字符数组，然后调用API排序，这样从前到后应该都是一样的。
```
class Solution {
    public boolean isAnagram(String s, String t) {
        if(s.length() != t.length()) return false;
        char[] chars = s.toCharArray();
        char[] chart = t.toCharArray();
        Arrays.sort(chars);
        Arrays.sort(chart);
        //或者可以直接调用比较方法判断两个数组是否相等；
        //Arrays.equals(chars,chart);
        int i = 0;
        while(i < s.length()){
            if(chars[i] != chart[i]) return false;
            else i++;
        }
        return true;
    }
}
```
### 解法二：哈希表
使用哈希表存储第一个字符串中每个字符出现的次数，然后遍历第二个字符串，如果出现新的，直接0-1，如果是有那个字符，那就那个字符的值-1，所以最后只要出现了-1那就false；
```
class Solution {
    public boolean isAnagram(String s, String t) {
        if(s.length() != t.length()) return false;
        Map<Character, Integer> map = new HashMap<>();
        for(Character c : s.toCharArray()){
            map.put(c, map.getOrDefault(c, 0)+1);
        }
        for(Character c : t.toCharArray()){
            map.put(c, map.getOrDefault(c, 0)-1); //有的话其值-1，没有的话直接-1；
            if(map.get(c) < 0) return false;
        }
        return true;
    }
}
```
