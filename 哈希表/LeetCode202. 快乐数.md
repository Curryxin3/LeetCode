## 202. 快乐数
> **知识点：set；双指针**
### 题目描述

编写一个算法来判断一个数 n 是不是快乐数。

「快乐数」定义为：

对于一个正整数，每一次将该数替换为它每个位置上的数字的平方和。
然后重复这个过程直到这个数变为 1，也可能是 无限循环 但始终变不到 1。
如果 可以变为  1，那么这个数就是快乐数。
如果 n 是快乐数就返回 true ；不是，则返回 false 。

##### 示例

```
输入：19
输出：true
解释：
12 + 92 = 82
82 + 22 = 68
62 + 82 = 100
12 + 02 + 02 = 1

输入：n = 2
输出：false
```
---
### 解法一：哈希集合
这个题目的关键在于知道要考的是什么，从这个题中抽出来关键信息：其实这样不断的求解过程中只可能出现三种情况：
- 1 最终得到1；
- 2 最终陷入了一个循环；
- 3 最终值越来越大，接近无穷大；
其实第三种发生不了，不同位数对应的下一位最大数字如下表

![image](https://note.youdao.com/yws/public/resource/806e27c4466efcf75e18b51ca94d39c9/xmlnote/9D3CA615062A4D3C815FC73F24B342CD/7756)
比如说4位或者更多位数的数字，在经过一轮以后就会回到3位上，也就是不会超过243.所以第三种情况不会存在，所以现在题目就变成了**我们会不会遇到循环：如果两个数字相等，就会出现循环**
- 判断前后两个数是否重复，这不就是set吗？因为set查找相等的复杂度为O(1);
- 判断能不能出现循环，想想链表里的快慢指针，龟兔赛跑； --> **快慢指针找循环**；
```
class Solution {
    public boolean isHappy(int n) {
        Set<Integer> set = new HashSet<>();
        while(n != 1 && !set.contains(n)){
            set.add(n);
            n = getNum(n);
        }
        return n==1;
    }
    private int getNum(int n){
        int sum = 0, d = 0;
        while(n != 0){
            d = n % 10;
            n = n / 10;
            sum += d*d;
        }
        return sum;
    }
}
```
### 解法二：快慢指针

```
class Solution {
    public boolean isHappy(int n) {
        int slow = n;
        int fast = getNum(n);
        while(fast != 1 && fast != slow){
            fast = getNum(getNum(fast)); //一次走两步；
            slow = getNum(slow);
        }
        return fast == 1;
    }
    private int getNum(int n){
        int sum = 0;
        while(n != 0){
            int d = n % 10;
            n = n / 10;
            sum += d*d;
        }
        return sum;
    }
}
```
时间复杂度
### 体会
快慢指针(龟兔赛跑)找循环
