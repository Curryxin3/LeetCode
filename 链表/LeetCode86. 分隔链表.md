## 86. 分隔链表
> **知识点：链表；**
### 题目描述

给你一个链表的头节点 head 和一个特定值 x ，请你对链表进行分隔，使得所有 小于 x 的节点都出现在 大于或等于 x 的节点之前。

你应当 保留 两个分区中每个节点的初始相对位置。

##### 示例
```
输入：head = [1,4,3,2,5,2], x = 3
输出：[1,2,2,4,3,5]。

输入：head = [2,1], x = 2
输出：[1,2]
```
---
### 解法一：解析
可以分别定义两个链表，一个用来存储比x大的，一个用来存储比x小的，然后把大的接到小的后面。里面的注意就是最后要防止不是以null结尾的，即最后一个元素原链条没有断，就会形成环；
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
    public ListNode partition(ListNode head, int x) {
        ListNode big = new ListNode(-1); //创建一个big链表；
        ListNode bigHead = big;
        ListNode small = new ListNode(-1); //创建一个small链表；
        ListNode smallHead = small;
        ListNode cur = head;
        while(cur != null){
            if(cur.val < x){
                small.next = cur; //小于的接到小链表上；大的接到大链表上；
                small = small.next;
            }else{
                big.next = cur;
                big = big.next;
            }
            cur = cur.next;
        }
        small.next = bigHead.next; //大链表接到小链表上；
        big.next = null;     
        //如果原链表最后一位是小的，那现在大链表的最后还指向之前的小元素，
        //这样就形成了环，所以将其设置为null；
        return smallHead.next;
    }
}
```
时间复杂度：O(N);
空间复杂度：O(1);
