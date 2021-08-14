## 203.移除链表元素
> **知识点：链表；双指针**
### 题目描述
给你一个链表的头节点 head 和一个整数 val ，请你删除链表中所有满足 Node.val == val 的节点，并返回 新的头节点 。
##### 示例
```
输入：head = [1,2,6,3,4,5,6], val = 6
输出：[1,2,3,4,5]

输入：head = [], val = 1
输出：[]

输入：head = [7,7,7,7], val = 7
输出：[]
```
---

### 解法一:迭代法
思路是很简单的，就是遍历链表，当遇到与val值相等的时候就将其前面节点直接指向后面节点，也就是直接忽略（隔掉）当前节点，所以可知得用两个指针一个保持当前节点，一个保持当前节点的上一个节点；并且最后返回的是ListNode，所以需要用一个虚拟头指针，里面的数据无所谓，主要是为了操作方便，使链表永不为空，永不无头等功能。
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
    public ListNode removeElements(ListNode head, int val) {
        //因为要删除一个元素就需要将链表的当前元素的上一元素的指针直接指向当前元素的下一元    素，
        //所以始终需要两个指针来保证获得当前元素和当前元素的上一元素；
        ListNode prev = new ListNode(-1);
        ListNode headNode = prev;   //定义一个虚拟头节点方便返回；
        ListNode cur = head;  //始终指向当前节点；
        prev.next = cur;      //虚拟头节点指向首节点；
        while(cur != null){
            if(cur.val == val){  //删除链表元素；
                prev.next = cur.next;
                cur = cur.next;
            }else{
                prev = cur;   //前后指针向后移；
                cur = cur.next;
            }
        }
        return headNode.next;
    }
}
```

时间复杂度：O(N);

### 体会
这里链表的第一道题，从中也可以看出一些对链表常用的一些操作：虚拟头节点；前一节点；当前节点；这都是需要在链表的问题中关注的。    
除此之外，还有明白这个代码中的关系，虽然在java中没有指针，但是要知道比如其中定义当前指针这句话在内存中是怎么存在的，知道为什么最后返回dummyNode.next；cur和pre都只是获得了一个地址值，都是对应的在内存中实际存在的一个链表进行的操作。
