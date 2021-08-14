## 56. 合并区间
> **知识点：贪心**
### 题目描述

以数组 intervals 表示若干个区间的集合，其中单个区间为 intervals[i] = [starti, endi] 。请你合并所有重叠的区间，并返回一个不重叠的区间数组，该数组需恰好覆盖输入中的所有区间。

##### 示例

```
输入：intervals = [[1,3],[2,6],[8,10],[15,18]]
输出：[[1,6],[8,10],[15,18]]
解释：区间 [1,3] 和 [2,6] 重叠, 将它们合并为 [1,6].

输入：intervals = [[1,4],[4,5]]
输出：[[1,5]]
解释：区间 [1,4] 和 [4,5] 可被视为重叠区间。

```
---
### 解法一：贪心

这也是**区间调度**的一类问题，在435题的时候我们的贪心策略是按照end进行排序，在这道题里，我们按照start进行排序。  

在遍历的过程中，如果当前元素的start>末尾元素的end，那就可以添加了，因为没重叠；如果当前的start<末尾元素，那就证明重叠了，这时候就需要更新最后元素的右边界，取原来右边界和当前元素右边界的大者。

```
class Solution {
    public int[][] merge(int[][] intervals) {
        Arrays.sort(intervals, new Comparator<int[]>(){
            public int compare(int[] o1, int[] o2){
                return o1[0]-o2[0];
            }
        });
        List<int[]> list = new ArrayList<>();
        for(int[] inter : intervals){
            int left= inter[0];
            int right = inter[1];
            if(list.size() == 0 || left > list.get(list.size()-1)[1]){
                list.add(inter);   //比最后一个的尾要大，不重叠，加入；
            }else{
                list.get(list.size()-1)[1] = Math.max(list.get(list.size()-1)[1], right); 
                //重叠的时候更新尾值；
            }
        }
        return list.toArray(new int[list.size()][]); 
    }
}
```
