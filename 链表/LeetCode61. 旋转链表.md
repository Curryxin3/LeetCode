## 61. 旋转链表
> **知识点：链表；**
### 题目描述

给你一个链表的头节点 head ，旋转链表，将链表每个节点向右移动 k 个位置。

##### 示例
```
输入：head = [1,2,3,4,5], k = 2
输出：[4,5,1,2,3]

输入：head = [0,1,2], k = 4
输出：[2,0,1]
```
---
### 解法一：解析
观察可以发现，右移k个节点其实就是以倒数第k个节点作为头节点，即正数第n-k+1个节点作为头节点；
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
    public ListNode rotateRight(ListNode head, int k) {
        //向右移动k个节点就是倒数第k个节点作为头节点；即正着数第n-k+1个；
        if(k == 0 || head == null) return head;
        ListNode cur = head;
        int n = 1;
        while(cur.next != null){
            cur = cur.next;
            n++;  //获得链表长度；
        }
        int m = n - k % n; //用来获取正着数节点在哪；
        cur.next = head; //闭合成环；
        while(m > 0){
            cur = cur.next;
            m--;  //指针停在新链表的尾结点；
        }
        ListNode newHaed = cur.next;
        cur.next = null; //断开环；
        return newHaed;
    }
}
```
时间复杂度：O(N);      
空间复杂度：O(1);   

### 体会
对于链表的while循环     
```
//1.如果最后我们只是想把链表遍历一遍，不需要最后那个指针； 
//cur最后停留在了最后的尾节点null上；
while(cur != null){
    ...
    cur = cur.next;
}
//2.如果我们想遍历一遍链表，而且最后一个指针我们之后还有用的；
//cur最后停留在了最后一个节点上。
while(cur.next != null){
    ...
    cur = cur.next;
    ...
}
```
获取链表的长度
```
//写法1：
int n = 1; //注意n从1开始。
while(cur.next != null){
    cur = cur.next;//指针最后在尾节点；
    n++;
}
//写法2：不用这种！
int n = 0;
while(cur != null){
    cur = cur.next; //指针停在null上；
    n++;
}
```
### 相关题目
[链表中倒数第k个节点](https://www.cnblogs.com/Curryxin/p/15038029.html)
### 参考链接
[旋转链表](https://leetcode-cn.com/problems/rotate-list/solution/xuan-zhuan-lian-biao-by-leetcode-solutio-woq1/)
