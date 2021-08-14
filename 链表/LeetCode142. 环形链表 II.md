## 142. 环形链表 II
> **知识点：链表；set；快慢指针**
### 题目描述
给定一个链表，判断链表中是否有环。

给定一个链表，返回链表开始入环的第一个节点。 如果链表无环，则返回 null。

为了表示给定链表中的环，我们使用整数 pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。 如果 pos 是 -1，则在该链表中没有环。注意，pos 仅仅是用于标识环的情况，并不会作为参数传递到函数中。

说明：不允许修改给定的链表。

如果链表中存在环，则返回 true 。 否则，返回 false 。     
**进阶：**     
你能用 O(1)（即，常量）内存解决此问题吗？
##### 示例
```
输入：head = [3,2,0,-4], pos = 1
输出：返回索引为 1 的链表节点
解释：链表中有一个环，其尾部连接到第二个节点。

输入：head = [1,2], pos = 0
输出：返回索引为 0 的链表节点
解释：链表中有一个环，其尾部连接到第一个节点。

输入：head = [1], pos = -1
输出：返回 null
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
    public ListNode detectCycle(ListNode head) {
        Set<ListNode> set = new HashSet<>();
        while(head != null){
            if(set.contains(head)){
                return head;
            }
            set.add(head);
            head = head.next;
        }
        return null;
    }
}
```
时间复杂度：O(N);
空间复杂度：O(N);
### 解法二：快慢指针
和141题一样，想一下，快指针每次走两步，慢指针每次走一步，如果设快指针走的路程是f，慢指针走的路程是s，那在相遇的时候，快指针正好是慢指针的两倍：f=2s；此外相遇时，快指针比慢指针多走了n圈，即f = s + nb, b 是一圈的长度，即得到这两个式子：       
- 1. f = 2s;
- 2. f = s + nb;       

推出；s = nb;        
走a+nb步一定是在链表入环口，slow已经走了nb了，再走a就可以了，怎么走a呢，让一个指针从头走，最后就可以在起点处相遇；
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
    public ListNode detectCycle(ListNode head) {
        ListNode fast = head;
        ListNode slow = head;
        while(fast != null && fast.next != null){
            fast = fast.next.next;
            slow = slow.next;
            if(fast == slow){
                fast = head;
                while(fast != slow){
                    fast = fast.next;
                    slow = slow.next;
                }
                return fast;
            }
        }
        return null;
    }
}
```
时间复杂度：O(N);
空间复杂度：O(1);
### 体会
双指针是一种很常用很常用的解题思路，其中有快慢指针这种，就是用来解决环、循环的问题；就是一个追及问题。
