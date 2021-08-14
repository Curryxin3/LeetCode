## 328. 奇偶链表
> **知识点：链表；双指针**
### 题目描述

给定一个单链表，把所有的奇数节点和偶数节点分别排在一起。请注意，这里的奇数节点和偶数节点指的是节点编号的奇偶性，而不是节点的值的奇偶性。

请尝试使用原地算法完成。你的算法的空间复杂度应为 O(1)，时间复杂度应为 O(nodes)，nodes 为节点总数。
    
**进阶：**     
你能用 O(1)（即，常量）内存解决此问题吗？
##### 示例
```
输入: 1->2->3->4->5->NULL
输出: 1->3->5->2->4->NULL。

输入: 2->1->3->5->6->4->7->NULL 
输出: 2->3->6->7->1->5->4->NULL。
```
---
### 解法一：双指针

分别维护两个指针，一个奇数指针，一个偶数指针，偶数指针每次都是奇数指针的后一位，注意迭代结束条件：even == null，或者even.next == null；然后将偶数链表接到奇数链表上就可以了。

```
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode oddEvenList(ListNode head) {
        if(head == null || head.next == null) return head;
        ListNode odd = head, even = head.next;
        ListNode evenHead = even;  //记录偶数头，方便一会连接；
        while(even != null && even.next != null){  //注意结束条件，画个图很明显；
            odd.next = even.next;
            odd = odd.next;
            even.next = odd.next;
            even = even.next;
        }
        odd.next = evenHead;
        return head;
    }
}
```
时间复杂度：O(N);
空间复杂度：O(1);
