## 136. 只出现一次的数字
> **知识点：哈希表；set；消消乐；位运算**
### 题目描述

给定一个非空整数数组，除了某个元素只出现一次以外，其余每个元素均出现两次。找出那个只出现了一次的元素

你的算法应该具有线性时间复杂度。 你可以不使用额外空间来实现吗？
##### 示例

```
输入: [2,2,1]
输出: 1

输入: [4,1,2,1,2]
输出: 4
```
---
### 解法一：哈希表
统计次数 --> 哈希表  
依次用哈希表存储每个数字出现的次数。 然后遍历哈希表看谁等于1就可以了。
```
class Solution {
    public int singleNumber(int[] nums) {
        Map<Integer, Integer> map = new HashMap<>();
        for(Integer i : nums){
            map.put(i, map.getOrDefault(i, 0)+1);
        }
        for(Integer i : map.keySet()){
            if(map.get(i) == 1) return i;
        }
        return -1;
    }
}
```
时间复杂度：O(N);   
空间复杂度：O(N);
### 解法二：排序搜索
直接排序，然后比较相邻的两个元素是否相等，每次走两步；
```
class Solution {
    public int singleNumber(int[] nums) {
        Arrays.sort(nums);
        for(int i = 0; i < nums.length-1; i=i+2){
            if(nums[i] != nums[i+1]) return nums[i];
        }
        return nums[nums.length-1]; //特殊情况，次数1的在最后；
    }
}
```
时间复杂度：O(N);
### 解法三：Set
遇到唯一这种字眼，下意识的就要想到set。如果遇到存在的，就移除，不存在的就加入，最后剩的就是了。
```
class Solution {
    public int singleNumber(int[] nums) {
        Set<Integer> set = new HashSet<>();
        for(Integer i : nums){
            if(set.contains(i)) set.remove(i);
            else set.add(i);
        }
        return set.iterator().next(); //遍历set（要会遍历）
    }
}
```
### 解法四：栈
通过栈去两两抵消：要入栈的元素等于栈顶元素，则弹出。否则入栈。   
**前提**：数组进行了排序。
```
class Solution {
    public int singleNumber(int[] nums) {
        Stack<Integer> stack = new Stack<>();
        Arrays.sort(nums);
        for(int i : nums){
            if(stack.isEmpty()){
                stack.push(i);
                continue;
            }
            if(stack.peek() != i){  //要入栈的和栈顶元素不同，则栈顶即为只出现一次的元素；
                return stack.peek();
            }
            stack.pop(); //两者一样时弹出，完成抵消；
        }
        return stack.peek(); //最后一个是只出现一次的。
    }
}
```
### 解法五：求和
将不重复的数字找出来*2-原数组就是要找的数字；
![image](https://note.youdao.com/yws/public/resource/806e27c4466efcf75e18b51ca94d39c9/xmlnote/1CD53EB52B9D4B408DAD6614F60487D6/7626)
```
class Solution {
    public int singleNumber(int[] nums) {
        Set<Integer> set = new HashSet<>();
        int sum1 = 0,sum2 = 0;
        for(Integer i : nums){
            if(!set.contains(i)){
                 set.add(i);
                 sum2 += i;
            }
            sum1 += i;
        }
        return 2*sum2-sum1;
    }
}
```
### 解法六：位运算

异或运算有几个很重要的性质：
> 1.任何数和0异或，仍为本身：a⊕0 = a      
2.任何数和本身异或，为0：a⊕a = 0       
3.异或运算满足交换律和结合律：a⊕b⊕a = (a⊕a)⊕b = 0⊕b = b

可以利用第2条和第3条性质将整个数组异或运算，那最后剩下的那个就是只出现一次的。
```
class Solution {
    public int singleNumber(int[] nums) {
        int ans = 0;
        for(Integer i : nums){
            ans ^= i;
        }
        return ans;
    }
}
```
### 体会

上面的解法整体上分为2种：一种就是将每个元素出现的次数统计出来，然后我们找到出现一次的；另一种就是将重复的两两抵消，那最后剩下的就是我们要的。其实方法2-6都是用的这种思路，两两抵消.剩下的就是。其中位运算是最快的，而且不用开辟新的空间。   


**掌握位运算的解决方法：这种题目往往要按位与、按位异或等操作**；   

异或运算有几个很重要的性质：
> 1.任何数和0异或，仍为本身：a⊕0 = a      
2.任何数和本身异或，为0：a⊕a = 0       
3.异或运算满足交换律和结合律：a⊕b⊕a = (a⊕a)⊕b = 0⊕b = b    

此外还会有左移<<;右移>>等；比如说：  
> a & 1 ： a的其他位全为0，最后一位不变：即取a最后一位；  
a | (1 << i) : a的其他位不变，把a的第i位置为1；  
(a >> i) & 1 : 取出a第i位上的值；

目前掌握的题型中触发位运算的是：题目中含有出现次数的问题；
### 相关题目   

[137.只出现一次的数字II](https://www.cnblogs.com/Curryxin/p/15053959.html);    

[260.只出现一次的数字III](https://www.cnblogs.com/Curryxin/p/15054052.html);  

[剑指 Offer 65. 不用加减乘除做加法](https://www.cnblogs.com/Curryxin/p/15139271.html)    

[剑指 Offer 15. 二进制中1的个数](https://www.cnblogs.com/Curryxin/p/15139285.html)    

[78. 子集](https://www.cnblogs.com/Curryxin/p/15139320.html)

### 相关链接
[求次数问题](https://leetcode-cn.com/problems/single-number/solution/dong-hua-dong-tu-yi-ding-hui-by-yuan-chu-vs4p/)
