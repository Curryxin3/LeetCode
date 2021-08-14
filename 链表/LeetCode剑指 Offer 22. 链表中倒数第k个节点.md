## 剑指 Offer 22. 链表中倒数第k个节点
> **知识点：链表；双指针**
### 题目描述

输入一个链表，输出该链表中倒数第k个节点。为了符合大多数人的习惯，本题从1开始计数，即链表的尾节点是倒数第1个节点。

例如，一个链表有 6 个节点，从头节点开始，它们的值依次是 1、2、3、4、5、6。这个链表的倒数第 3 个节点是值为 4 的节点。
。

##### 示例
```
给定一个链表: 1->2->3->4->5, 和 k = 2.

返回链表 4->5.
```
---
### 解法一：解析
这题和leetcode 61 旋转链表很像，都是关于倒数的问题，可以发现：倒数第k个节点就是正数第n-k+1个节点，头指针移动n-k次。
所以需要获取链表长度；
```
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode getKthFromEnd(ListNode head, int k) {
        //method1:统计链表长度
        //倒数第k个就是正数第n-k+1个；移动n-k次；
        ListNode cur = head;
        int n = 1;
        while(cur.next != null){ //获取链表长度；
            cur = cur.next;
            n++;
        }
        int m = n-k;
        while(m > 0){
            head = head.next;
            m--;
        }
        return head;
    }
}
```
时间复杂度：O(N);
空间复杂度：O(1);
### 解法二：双指针
上面最坏情况下需要将链表遍历两次，一次能解决吗？这时候就要双指针了，想一下，我们可以构建两个间隔是k的指针，这样前面的指针走到末尾时，后面的指针正好就是倒数第k个。
```
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode getKthFromEnd(ListNode head, int k) {
        ListNode front = head;
        ListNode later = head;
        while(k>0){  //前指针距后指针k；
            front = front.next;
            k--;  
        }
        while(front != null){
            front = front.next;
            later = later.next;
        }
        return later;
    }
}
```
时间复杂度；O(N);       
空间复杂度：O(1);
