## 69. x 的平方根
> **知识点：二分查找；数学**

### 题目描述

实现 int sqrt(int x) 函数。

计算并返回 x 的平方根，其中 x 是非负整数。

由于返回类型是整数，结果只保留整数的部分，小数部分将被舍去。

##### 示例

```
输入: 4
输出: 2

输入: 8
输出: 2
说明: 8 的平方根是 2.82842..., 
     由于返回类型是整数，小数部分将被舍去。
```
---
### 解法一：二分查找；   

由于此题是只返回整数部分就可以，所以可以使用二分查找，也就是找到k^2 <= x 的最大k值；所以可以使用二分查找来得到答案；   
**注意细节**
```
class Solution {
    public int mySqrt(int x) {
        if(x == 0) return 0;
        int left = 0, right = x, ans = 0;
        while(left <= right){
            int mid = left + ((right-left) >> 1);
            if((long)mid * mid <= x){
                ans = mid;
                left = mid+1;
            }else{
                right = mid-1;
            }
        }
        return ans;
    }   
}
```

### 解法二：数学

这道题目可以使用牛顿迭代法，用来快速求零点。求一个数的平方根，其实就是求x^2-C的零点，所以可以从一个初始点开始，不断的向零点进行逼近。    
任取一个初始点x0，找到它在图像中的位置，然后对其做切线，求切线的零点xi，那这个xi比x0更靠近零点；然后这样不断迭代，知道满足某精度；    

![image](https://note.youdao.com/yws/public/resource/4f8a62f486551811aaff90f848106058/xmlnote/E37CC729DD5E4C3895E0E19850BE3045/13520)

根据数学可以求出这条切线的左边，也就是xi的值；  

0.5 * (x0 + C / xo);

```
class Solution {
    public int mySqrt(int x) {
        if(x == 0) return 0;
        double C = x, x0 = x;
        while(true){
            double xi = 0.5*(x0 + C / x0);
            if(Math.abs(x0 - xi) < 1e-7){
                break;
            }
            x0 = xi;
        }
        return (int)x0;
    }
}
```

### 相关链接  

[x的平方根](https://leetcode-cn.com/problems/sqrtx/solution/x-de-ping-fang-gen-by-leetcode-solution/)
