## 141. 环形链表
> **知识点：链表；集合；快慢指针**
### 题目描述
给定一个链表，判断链表中是否有环。

如果链表中有某个节点，可以通过连续跟踪 next 指针再次到达，则链表中存在环。 为了表示给定链表中的环，我们使用整数 pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。 如果 pos 是 -1，则在该链表中没有环。注意：pos 不作为参数进行传递，仅仅是为了标识链表的实际情况。

如果链表中存在环，则返回 true 。 否则，返回 false 。     
**进阶：**     
你能用 O(1)（即，常量）内存解决此问题吗？
##### 示例
```
输入：head = [3,2,0,-4], pos = 1
输出：true
解释：链表中有一个环，其尾部连接到第二个节点。

输入：head = [1,2], pos = 0
输出：true
解释：链表中有一个环，其尾部连接到第一个节点。

输入：head = [1], pos = -1
输出：false
解释：链表中没有环。
```
---
### 解法一：集合
利于集合不重复的特性，依次将节点存入集合；
```
/**
 * Definition for singly-linked list.
 * class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public boolean hasCycle(ListNode head) {
        //法一：集合
        Set<ListNode> set = new HashSet<>();
        while(head != null){
            if(set.contains(head)) return true;
            set.add(head);
            head = head.next;
        }
        return false;
    }
}
```
时间复杂度：O(N);
空间复杂度：O(N);
### 解法二：快慢指针
快慢指针就是用来处理环的问题的，如果有环，因为快指针每次相对慢指针移动1次，相当于逐渐靠近慢指针，所以最终一定会碰面；(想象一下在操场跑步，跑的快的一定会追上跑的慢的)
```
/**
 * Definition for singly-linked list.
 * class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public boolean hasCycle(ListNode head) {
        //method 2:快慢指针；
        ListNode fast = head;
        ListNode low = head;
        while (fast != null && fast.next != null){
            fast = fast.next.next;
            low = low.next;
            if (fast == low){
                return true;
            }
        }
        return false;
    }
}
```
时间复杂度：O(N);
空间复杂度：O(1);
### 体会
双指针是一种很常用很常用的解题思路，其中有快慢指针这种，就是用来解决环、循环的问题；就是一个追及问题。     

### 相关题目   
[142. 环形链表 II](https://www.cnblogs.com/Curryxin/p/15072144.html)
