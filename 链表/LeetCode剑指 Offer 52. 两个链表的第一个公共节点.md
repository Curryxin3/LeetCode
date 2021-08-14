## 剑指 Offer 52. 两个链表的第一个公共节点
> **知识点：链表；**
### 题目描述

输入两个链表，找出它们的第一个公共节点。

如下面的两个链表：

##### 示例
示例1：
![image](https://note.youdao.com/yws/public/resource/57efe0611c0bb7f2d3c35ca50fcaa18c/xmlnote/4B314CEF6D1B4B728697B2A2A6884A38/8305)
```
输入：intersectVal = 8, listA = [4,1,8,4,5], listB = [5,0,1,8,4,5], skipA = 2, skipB = 3
输出：Reference of the node with value = 8
输入解释：相交节点的值为 8 （注意，如果两个列表相交则不能为 0）。从各自的表头开始算起，链表 A 为 [4,1,8,4,5]，链表 B 为 [5,0,1,8,4,5]。在 A 中，相交节点前有 2 个节点；在 B 中，相交节点前有 3 个节点。
```
示例2：
![image](https://note.youdao.com/yws/public/resource/57efe0611c0bb7f2d3c35ca50fcaa18c/xmlnote/4B0E9A9741C74395A7690FCCFC4952F0/8308)
```
输入：intersectVal = 2, listA = [0,9,1,2,4], listB = [3,2,4], skipA = 3, skipB = 1
输出：Reference of the node with value = 2
输入解释：相交节点的值为 2 （注意，如果两个列表相交则不能为 0）。从各自的表头开始算起，链表 A 为 [0,9,1,2,4]，链表 B 为 [3,2,4]。在 A 中，相交节点前有 3 个节点；在 B 中，相交节点前有 1 个节点。
```
---
### 解法一：解析
我们可以假设链表A独有部分长度为m，链表B独有部分长度为n，两个链表相交部分长度为x，所以链表A的长度为m+x，链表B的长度为n+x。我们定义两个指针从两个链表同时走，A走完后去走B，B走完后去走A，两者速度相同，最后到达相交点处正好碰面。走的距离都是m+n+x；
```
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        if(headA == null || headB == null) return null;
        ListNode tempA = headA;
        ListNode tempB = headB;
        while(tempA != tempB){
            //A走到头就接到B上；
            tempA = tempA != null ? tempA.next : headB;
            //B走到头就接到A上；
            tempB = tempB != null ? tempB.next : headA;
        }
        return tempB;
    }
}
```
时间复杂度：O(N);      
空间复杂度：O(1);   
