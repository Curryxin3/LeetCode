## 206.反转链表
> **知识点：链表；双指针；**
### 题目描述
给你单链表的头节点 head ，请你反转链表，并返回反转后的链表。
##### 示例
```
输入：head = [1,2,3,4,5]
输出：[5,4,3,2,1]

输入：head = [1,2]
输出：[2,1]

输入：head = []
输出：[]
```
---
### 解法一：双指针
将链表反转，就是把箭头的顺序换一下就可以了，可以采用双指针的方法，让cur的next为pre，然后pre和cur逐渐遍历，直到最后；
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
    public ListNode reverseList(ListNode head) {
        ListNode pre = null;
        ListNode cur = head;
        while(cur != null){
            ListNode temp = cur.next; //临时存储当前的下一节点，要不断了就找不到了；
            cur.next = pre;  //往前指
            pre = cur;       //指针往后移；
            cur = temp;
        }
        return pre;
    }
}
```
时间复杂度：O(N);
### 体会
链表的题一般都需要定义虚头，并且格外关注前一节点和后一节点。
