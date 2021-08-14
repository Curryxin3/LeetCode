## 【二分查找】35. 搜索插入位置

> **知识点：数组，二分查找**；
### 题目描述

给定一个排序数组和一个目标值，在数组中找到目标值，并返回其索引。如果目标值不存在于数组中，返回它将会被按顺序插入的位置。

请必须使用时间复杂度为 O(log n) 的算法。

##### 示例

```
输入: nums = [1,3,5,6], target = 5
输出: 2

输入: nums = [1,3,5,6], target = 2
输出: 1

输入: nums = [1,3,5,6], target = 7
输出: 4
```
---

### 解法一：二分查找

这道题目就是典型的二分搜索。下面介绍一个重要的查找方法，二分查找。   
#### 二分查找    
可查看二分查找总结 ：[二分查找](https://www.cnblogs.com/Curryxin/p/15103033.html)  

如果想要在数组中查找一个数，最基本的方法就是暴力解法：一次遍历，这时候时间复杂度是O(N),二分查找就是其中的一种优化，时间复杂度是O(logN);具体做法是一步一步逼近直到找到。**前提是数组需要是一个排序数组**。     
Knuth 大佬（发明 KMP 算法的那位）怎么说的：
Although the basic idea of binary search is comparatively straightforward, the details can be surprisingly tricky...   
思路很简单，**细节是魔鬼**；   

二分查找的模板：  
```
//循环写法；
public int binarySearch(int[] nums, int target){
    int left = 0, right = num.length-1;
    while(left <= right){  //细节1：循环条件；
        int mid = left + ((right-left) >> 1);  
        //细节2：防止溢出，此外需要注意由于优先级的原因，需要添加括号；
        if(nums[mid] == target){
            return mid;
        }else if(nums[mid] < target){
            left = mid + 1; //细节3：注意加减1；
        }else{
            right = mid - 1;
        }
    }
    return -1
}

//递归写法：
public int binarySearch(int[] nums, int target, int left, int right){
    if(left <= right){
        int mid = left + ((right-left)-1);
        if(nums[mid] == target){
            return mid;  //查找成功；
        }else if(nums[mid] > target){
            return binarySearch(nums, target, left, mid-1); //新的区间：左半区间；
        }else{
            return binarySearch(nums, target, min+1, right); //新的区间: 右半区间；
        }
    }
    return -1;
}
```
上述功能就是如果能在数组中找到目标值，就返回其索引，如果找不到，就返回其下标；如果目标值比中间值还大，那肯定在中间值右侧(因为数组已经排序好了)，如果目标值比mid值小，那肯定在mid左侧。  
- 细节1：为什么while循环的条件时<=?    
因为我们初始化的时候右侧区间是nums.length-1;所以是包括right的，即我们的区间是[left,right],这样一个左闭右闭的区间，把这个区间理解成**搜索区间**，即我们是在这样一个区间上搜索，那什么时候停止呢，两个原因：
   - 1.找到了目标值，那就停止；  
   - 2.没找到目标值，但是搜索区间为空了，没得找了，这时候停止；  
 所以在最后一个=的时候，比如[2,2]这时候区间还不为空，万一就是这个2呢。  
- 细节2：为什么写成left+((right-left) >> 1);  
这主要是为了防止溢出，记住就可以了，注意除以2,用位运算的话会比较快一点，而且记得带外面那个大括号；  
- 细节3： 为什么left = mid + 1，right = mid - 1？    
想一下刚才搜索区间的概念，如果发现了索引mid不是要找的target，那自然要从将mid剔除掉，从mid的左边或者右边找起来了。   

缺陷：上述算法存在一个缺陷就是不能返回左右侧边界，比如数组是[1,2,2,2,3], target是2，这时候返回的索引是2，没有办法返回左右边界。

---

好了，现在回到本题，本题和标准二分查找唯一区别就是如果找不到的时候返回它应该在的位置,，**这个应该在的位置其实就是第一个比target大的元素位置**，其实，和上面的程序一模一样，唯一的区别就是最后不再返回-1，而返回left。left就一定是最后应该插入的位置吗？在上述过程中，如果找到了，那就直接返回；如果原数组中不包含target，**那while最后一次执行的一定是left=right=mid,这时候mid左边的数全部小于target，mid右边的数全部大于target，所以此时返回的插入位置分为两种情况**：  
- 1.nums[mid] > target, 此时执行right=mid-1，直接返回mid也就是left就行。 
- 2.nums[mid] < target,此时执行left=mid+1,这时候就到了第一个比target大的值，返回left就可以了；

```
class Solution {
    public int searchInsert(int[] nums, int target) {
        int left = 0, right = nums.length-1;
        while(left <= right){ //细节1
            int mid = left + ((right-left) >> 1); 
            if(nums[mid] == target) return mid;
            else if(nums[mid] > target){
                right = mid-1;
            }else{
                left = mid+1;
            }
        }
        return left;
    }
}
```
时间复杂度：O(logN);

### 体会
- 注意查看上面中提到的二分法的模板和细节，注意什么时候带等号，什么时候不带等号；  
- 二分查找本质上就是一个不断缩小搜索空间的过程，比如我们找出某个值，就是在不断的把空间缩小，找出哪个数字乱序了，也在不断的把空间缩小，求一个数的开根号，不断的缩小空间然后拿值去逼近。这个过程要多想一下，**就是我们不断的缩小空间，然后每次都是拿这个空间上的中间值去做某种判断，然后去逼近我们的结果**。
- 二分查找应用的前提就是一定是一个有序的，或者半有序的，如果是半有序的话原则是就是不断向有序上去赶，因为只有在有序的时候才能去二分；
- 上面提供的二分查找的模板要始终明白在最后一步跳出循环前两点：
   - 1.最后一步一定是right=left=mid。
   - 2.mid左侧和右侧一定是已经有了规律的了。比如查找值的时候，mid左侧都比t值小，mid右侧都比t值大；比如判断从哪个数字开始乱了，mid左侧一定是整齐的，mid右侧一定是乱的。那这时候只要去判断一下mid的值就可以了。如果mid>t或者说mid乱了，那正好返回left也就是mid，如果mid<t或者说mid没乱，那left=mid+1，这不就正好是第一个大于t或者第一个乱的了吗。

### 相关题目

[34. 在排序数组中查找元素的第一个和最后一个位置](https://www.cnblogs.com/Curryxin/p/15101136.html)

[33. 搜索旋转排序数组](https://www.cnblogs.com/Curryxin/p/15101199.html)    

[81. 搜索旋转排序数组 II](https://www.cnblogs.com/Curryxin/p/15101799.html)   

[153. 寻找旋转排序数组中的最小值](https://www.cnblogs.com/Curryxin/p/15101952.html)     

[74. 搜索二维矩阵](https://www.cnblogs.com/Curryxin/p/15102004.html)   

[剑指 Offer 53 - I. 在排序数组中查找数字 I](https://www.cnblogs.com/Curryxin/p/15102135.html)

[剑指 Offer 53 - II. 0～n-1中缺失的数字](https://www.cnblogs.com/Curryxin/p/15102217.html)

### 相关链接

[一文搞懂二分查找多题](https://leetcode-cn.com/problems/search-insert-position/solution/yi-wen-dai-ni-gao-ding-er-fen-cha-zhao-j-69ao/)
