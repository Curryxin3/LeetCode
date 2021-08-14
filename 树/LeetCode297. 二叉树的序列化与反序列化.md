## 297. 二叉树的序列化与反序列化
> **知识点：二叉树；递归**
### 题目描述

序列化是将一个数据结构或者对象转换为连续的比特位的操作，进而可以将转换后的数据存储在一个文件或者内存中，同时也可以通过网络传输到另一个计算机环境，采取相反方式重构得到原数据。

请设计一个算法来实现二叉树的序列化与反序列化。这里不限定你的序列 / 反序列化算法执行逻辑，你只需要保证一个二叉树可以被序列化为一个字符串并且将这个字符串反序列化为原始的树结构。

提示: 输入输出格式与 LeetCode 目前使用的方式一致，详情请参阅 LeetCode 序列化二叉树的格式。你并非必须采取这种方式，你也可以采用其他的方法解决这个问题。

##### 示例
![image](https://note.youdao.com/yws/public/resource/6fa0eca998f3cbca5812e4ebbe017e5e/xmlnote/EA6F3A4490C04DBDBAACAA160FE1B613/11538)
```
输入：root = [1,2,3,null,null,4,5]
输出：[1,2,3,null,null,4,5]

输入：root = []
输出：[]

输入：root = [1]
输出：[1]]

输入：root = [1,2]
输出：[1,2]
```
---
### 解法一：递归

**序列化**
函数功能：将二叉树序列化  
1.终止条件：root == null, 返回"none,";  
2.该做什么：想一层节点该做什么：将节点的值加入字符串，然后把子树递归。  
3.什么时候做：自己先加入，再加子树，先序。  
**反序列化**  
函数功能：将字符串反序列化  
1.终止条件：如果数组的索引超出了长度，返回null；  
2.该做什么：想一层节点该做什么：如果字符值是"none"，返回null，如果不是，就以该值建立node，然后再去建立左子树，右子树。  
3.什么时候做：数组的索引越来越大，因为前面先序的原因，第一个是根，所以先序。

```
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Codec {
    // Encodes a tree to a single string.
    public String serialize(TreeNode root) {
        StringBuilder sb = new StringBuilder();
        if(root == null){
            sb.append("none,");
            return sb.toString();
        }
        sb.append(root.val+",");
        sb.append(serialize(root.left));
        sb.append(serialize(root.right));
        return sb.toString();
    }

    // Decodes your encoded data to tree.
    public TreeNode deserialize(String data) {
        String[] str = data.split(","); //分割成数组；
        Queue<String> queue = new LinkedList<>(Arrays.asList(str)); //全部入队；
        return deserialize(queue);
    }
    private TreeNode deserialize(Queue<String> queue){
        String head = queue.poll();
        if("none".equals(head)){
            return null;
        }
        TreeNode root = new TreeNode(Integer.valueOf(head));
        root.left = deserialize(queue);
        root.right = deserialize(queue);
        return root;
    }
}

// Your Codec object will be instantiated and called as such:
// Codec ser = new Codec();
// Codec deser = new Codec();
// TreeNode ans = deser.deserialize(ser.serialize(root));
```
空间复杂度：O(N):堆栈的支出
