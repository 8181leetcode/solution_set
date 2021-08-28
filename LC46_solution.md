# #LC46 全排列(Permutations)

## 题目描述：

给定一个不含重复数字的数组 `nums` ，返回其 **所有可能的全排列** 。你可以 **按任意顺序** 返回答案。

提示：

- `1 <= nums.length <= 6`
- `-10 <= nums[i] <= 10`
- `nums` 中的所有整数 **互不相同**

## 思路一：回溯+标记数组

本题是一道经典的回溯算法问题，assign本题的初衷就是复习回溯算法问题的模板并重温算法中的一些小细节。由于本题需要穷举所有可能，每次遍历都需要重新开始搜索，所以需要额外设置一个标记数组来记录每次搜索时path中的元素是否已经出现，因为每个元素只能被使用一次。算法的时间复杂度为*O*(*n*×*n*!)，空间复杂度*O*(*n*×*n*!)。为代码如下：

**代码(Java)**

```java
class Solution {
    public List<List<Integer>> permute(int[] nums) {
        int n = nums.length;
      	//细节1：记得判断数组长度为0的情况
        if(n==0) return null;
        List<List<Integer>> ans = new ArrayList<>();
        List<Integer> path = new ArrayList<>();
        boolean[] used = new boolean[n];
      	//细节2：回溯问题模板的基本参数(原数组，总深度，当前深度，总路径，当前路径...等)
        dfs(nums,0,n,used,path,ans);
        return ans;
    }
    public void dfs(int[] nums,int level,int len,boolean[] used,List<Integer> path,List<List<Integer>> ans){
      	//当搜索深度达到目标时加入新的结果
        if(level == len){
            ans.add(new ArrayList<Integer>(path));
            //细节3：注意引用和值的区别，下面的写法只是传递了一个引用在搜索结束时会被销毁
            //ans.add(path);
            return;
        }
        for(int i = 0;i < len;i++){
            if(!used[i]){
                path.add(nums[i]);
                used[i] = true;
                dfs(nums,level+1,len,used,path,ans);
                //每次搜索结束后恢复状态
                used[i] = false;
                path.remove(path.size()-1);
            }
        }
    }
}
```

## 思路二：回溯+交换位置

思路一中我们才有了标记数组来记录每个元素是否被使用，如果不考虑答案是否要按照字典序的话也可以在每次搜索的时候将数组的元素交换，使数组的左半部分为已经填过的数，右半部分为未填过的数。

**代码(Java)**

```java
class Solution {
    public List<List<Integer>> permute(int[] nums) {
        List<List<Integer>> res = new ArrayList<List<Integer>>();

        List<Integer> output = new ArrayList<Integer>();
        for (int num : nums) {
            output.add(num);
        }

        int n = nums.length;
        backtrack(n, output, res, 0);
        return res;
    }

    public void backtrack(int n, List<Integer> output, List<List<Integer>> res, int first) {
        // 所有数都填完了
        if (first == n) {
            res.add(new ArrayList<Integer>(output));
        }
        for (int i = first; i < n; i++) {
            // 动态维护数组
            Collections.swap(output, first, i);
            // 继续递归填下一个数
            backtrack(n, output, res, first + 1);
            // 撤销操作
            Collections.swap(output, first, i);
        }
    }
}
```

## 总结

全排列(Permutations)问题是一道经典的回溯算法问题，希望通过完成这道题可以复习回溯算法的思想，更好地理解递归过程。
