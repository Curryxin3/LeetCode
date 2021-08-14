## 3. 无重复字符的最长子串

> **知识点：字符串，滑动窗口**；
### 题目描述

给定一个字符串 s ，请你找出其中不含有重复字符的 最长子串 的长度。

##### 示例

```
输入: s = "abcabcbb"
输出: 3 
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。

输入: s = "bbbbb"
输出: 1
解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。

输入: s = "pwwkew"
输出: 3
解释: 因为无重复字符的最长子串是 "wke"，所以其长度为 3。
     请注意，你的答案必须是 子串 的长度，"pwke" 是一个子序列，不是子串。

输入: s = ""
输出: 0
```
---

### 解法一：滑动窗口

滑动窗口其实就是一种队列；    

对于本题，依次入队，当发现这个队列中含有重复元素的时候，把重复元素之前的值全部移除。比如 a b c a b;到第二个a的时候将a出队；    
如果不用队怎么实现，其实就是维持了一个滑动着的窗口，右边界依次在遍历，左边界也在移动，而且注意左边界一定不会回退的。当遍历到的元素出现过了的时候，要把左边界移动；    

其次不能有重复元素 -- > 哈希表；  

用哈希表存储每个字符和字符出现的索引，当出现重复字符的时候，移动左边界；注意两种情况：    
- a b c a；  第二个a的时候要把左边界移动到a的下一个处，相当于a出队；
- a b b a; 到达第2个b的时候因为之前含有b了，所以map.get(b)+1,也就是左边界到了2，但是到下一个a的时候，如果还用同样的，那left =  map.get(a)+1 = 1,就又回去了，所以应该取两者中的大值；相当于a b出队；出去的可千万别再进来了；滑动窗口的左右边界是永远不会回退的。  

**一句话：没有触发条件，右窗口一直移动；达到触发条件，左窗口移动，促使条件再次不满足；**，在这个题里，条件就是窗口内是否有重复字符；
```
class Solution {
    public int lengthOfLongestSubstring(String s) {
        int len = s.length();
        int maxLen = 0;
        int left = 0;
        Map<Character, Integer> map = new HashMap<>(); //为了检查重复，存储字符和其出现的索引；
        for(int i = 0; i < len; i++){
            if(map.containsKey(s.charAt(i))){
                left = Math.max(left, map.get(s.charAt(i))+1); //左窗口移动；左右边界永不回头；
            }
            map.put(s.charAt(i), i);
            maxLen = Math.max(maxLen, i-left+1);
        }
        return maxLen;
    }
}
```  

### 相关链接   

[无重复字符的最长子串](https://leetcode-cn.com/problems/longest-substring-without-repeating-characters/solution/hua-dong-chuang-kou-by-powcai/)


