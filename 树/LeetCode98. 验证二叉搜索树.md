## 98. 验证二叉搜索树
> **知识点：二叉树；递归**
### 题目描述

给定一个二叉树，判断其是否是一个有效的二叉搜索树。

假设一个二叉搜索树具有如下特征：

节点的左子树只包含小于当前节点的数。
节点的右子树只包含大于当前节点的数。
所有左子树和右子树自身必须也是二叉搜索树。

##### 示例

```
输入:
    2
   / \
  1   3
输出: true

输入:
    5
   / \
  1   4
     / \
    3   6
输出: false
解释: 输入为: [5,1,4,null,null,3,6]。
     根节点的值为 5 ，但是其右子节点值为 4 。
```
---
### 解法一：递归
注意二叉搜索树的含义：每个节点的左子树都小于节点值，每个节点的右子树都大于节点值。   
注意以下程序是错的：
```
class Solution {
    public boolean isValidBST(TreeNode root) {
        if(root == null) return true;
        if(root.left.val > root.val) return false;
        if(root.right.val < root.val) return false;
        return isValidBST(root.left) && isValidBST(root.right);
    }
}
```
上面程序只检验了每个树的左节点是否小于节点，右节点是否小于节点，比如示例2.不是二叉搜索树，但是会返回true。  
每个子树都满足，说明**中序遍历**的结果(左中右)是递增的序列，所以只要判断后一个序列比前一个序列大即可；中序遍历可用递归或迭代法实现。  
函数功能：判断是否是二叉搜索树  
1.终止条件：root == null, 返回true；  
2.该做什么：判断当前节点和前面节点的值的大小，如果比之前的值小了就直接返回false；  
3.什么时候做：就是从中序遍历的结果上比较大小的，中序。
```
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    long pre = Long.MIN_VALUE; //先定义一个最小值，注意要在方法外，不然每次递归就又重置了。
    public boolean isValidBST(TreeNode root) {
        if(root == null) return true;
        if(!isValidBST(root.left)) return false;
        if(root.val <= pre) return false;
        pre = root.val; //更新前一个元素的值；
        return isValidBST(root.right);
    }
}
```
空间复杂度：O(N):堆栈的支出
### 解法二：迭代

```
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    long pre = Long.MIN_VALUE;
    public boolean isValidBST(TreeNode root) {
        Stack<TreeNode> stack = new Stack<>();
        while(!stack.isEmpty() || root != null){
            if(root != null){
                stack.push(root);  //左边界全部入栈；
                root = root.left;  
            }else{
                TreeNode top = stack.pop();
                if(top.val <= pre){
                    return false;
                }
                root = top.right; //来到弹出节点的右节点；
                pre = top.val;  //更新上个元素的值；       
            }
        }
        return true;
    }
}
```
