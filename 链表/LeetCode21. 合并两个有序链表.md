## 21. 合并两个有序链表
> **知识点：链表**；
### 题目描述

将两个升序链表合并为一个新的 升序 链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。
##### 示例

```
输入：l1 = [1,2,4], l2 = [1,3,4]
输出：[1,1,2,3,4,4]

输入：l1 = [], l2 = []
输出：[]

输入：l1 = [], l2 = [0]
输出：[0]
```
---
### 解法一：迭代法
注意到两个链表已经是升序排好的，所以可以分别用两个指针定位到两个链表元素，然后依次比较大小，依次往后接，如果某个链表到尾了，那就直接把剩下的那个链表接上来接可以了；
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
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        ListNode headNode = new ListNode(-1);  //定义一个虚拟头节点，方便最后返回；
        ListNode headTemp = headNode; //负责移动；
        while(l1 != null && l2 != null){
            if(l1.val > l2.val){
                headTemp.next = l2;  //headtemp下一个接小的节点；
                l2 = l2.next;        //两个链表的指针后移；
            }else{
                headTemp.next = l1;
                l1 = l1.next;
            }
            headTemp = headTemp.next; //新链表的指针后移；
        }
        headTemp.next = l1 == null ? l2 : l1; //查看哪个链表到尾了，就把另一个链表接到后面；
        return headNode.next;
    }
}
```
时间复杂度:O(M+N);
### 解法二：递归

```

```
时间复杂度：
### 体会
链表主要注意指针的移动和虚拟头节点的定义；
### 参考
[合并两个排序列表](https://hub.fastgit.org/chefyuan/algorithm-base/blob/main/animation-simulation/%E9%93%BE%E8%A1%A8%E7%AF%87/%E5%89%91%E6%8C%87Offer25%E5%90%88%E5%B9%B6%E4%B8%A4%E4%B8%AA%E6%8E%92%E5%BA%8F%E7%9A%84%E9%93%BE%E8%A1%A8.md)
