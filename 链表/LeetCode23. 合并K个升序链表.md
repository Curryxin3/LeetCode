## 23. 合并K个升序链表
> **知识点：链表；递归；分治；堆**；
### 题目描述

给你一个链表数组，每个链表都已经按升序排列。

请你将所有链表合并到一个升序链表中，返回合并后的链表。
##### 示例

```
输入：lists = [[1,4,5],[1,3,4],[2,6]]
输出：[1,1,2,3,4,4,5,6]
解释：链表数组如下：
[
  1->4->5,
  1->3->4,
  2->6
]
将它们合并到一个有序链表中得到。
1->1->2->3->4->4->5->6。

输入：lists = []
输出：[]

输入：lists = [[]]
输出：[]
```
---
### 解法一：分治

首先，很容易的想到21题，合并两个有序链表，当时是采用双指针，依次从两个链表的头开始遍历，谁小先接上谁，然后依次往后接。     

所以这道题目可以直接将前两个链表进行合并，合完之后拿新链表和下一个接着合并，这种时间复杂度很高，O(K^2N);     

所以可以采用分治的方法，将k个链表中两两进行合并，而不是刚才的顺序合并，这样就可以将顺序的k复杂度减为logK。

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
    public ListNode mergeKLists(ListNode[] lists) {
        if(lists == null || lists.length == 0) return null;
        return merge(lists, 0, lists.length-1);
    }
    //将链表数组分治；
    private ListNode merge(ListNode[] list, int left, int right){
        if(left == right) return list[left];
        int mid = left + ((right-left) >> 1); 
        ListNode lnode = merge(list, left, mid); 
        ListNode rnode = merge(list, mid+1, right);
        return mergeTwoList(lnode, rnode);
    }
    //合并两个链表；
    private ListNode mergeTwoList(ListNode lnode, ListNode rnode){
        if(lnode == null || rnode == null) return lnode != null ? lnode : rnode;
        ListNode head = new ListNode(0);
        ListNode cur = head, left = lnode, right = rnode;
        while(left != null && right != null){
            if(left.val < right.val){
                cur.next = left;
                left = left.next;
            }else{
                cur.next = right;
                right = right.next;
            }
            cur = cur.next;
        }
        cur.next = (left != null ? left : right);
        return head.next;
    }
}
```

### 解法二：堆  

可以创建一个小根堆，将三个链表的头入堆，然后堆顶就是最小的，接着将其弹出，接到答案链表上，然后将此节点的下一个节点入堆，这样直到最后。

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
    public ListNode mergeKLists(ListNode[] lists) {
        PriorityQueue<ListNode> queue = new PriorityQueue<>(new Comparator<>(){
            public int compare(ListNode o1, ListNode o2){
                return o1.val-o2.val;  
            }
        });  //创建一个容量为k的小根堆；
        for(ListNode cur : lists){
            if(cur != null){
                queue.offer(cur);  //将各链表头节点入堆；
            }
        }
        ListNode head = new ListNode(0);
        ListNode cur = head;
        while(!queue.isEmpty()){
            ListNode min = queue.poll();
            cur.next = min;
            cur = cur.next;
            if(min.next != null){
                queue.offer(min.next);
            }
        }
        return head.next;
    }
}
```


### 相关题目

[21. 合并两个有序链表](https://www.cnblogs.com/Curryxin/p/15032593.html)

### 参考链接

[合并两个排序列表](https://hub.fastgit.org/chefyuan/algorithm-base/blob/main/animation-simulation/%E9%93%BE%E8%A1%A8%E7%AF%87/%E5%89%91%E6%8C%87Offer25%E5%90%88%E5%B9%B6%E4%B8%A4%E4%B8%AA%E6%8E%92%E5%BA%8F%E7%9A%84%E9%93%BE%E8%A1%A8.md)
