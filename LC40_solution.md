# #LC40 组合综合II

## 题目描述：

给定一个数组 candidates 和一个目标数 target ，找出 candidates 中所有可以使数字和为 target 的组合。

candidates 中的每个数字在每个组合中只能使用一次。

提示：

- `1 <= candidates.length <= 100`
- `1 <= candidates[i] <= 50`
- `1 <= target <= 30`

## 思路：使用剪枝的回溯算法

由于我们的目标是给出所有组合之和为target的数组，并且每个数字只能使用一次，所以比较容易想到回溯的方法来解决本问题。但是，由于本问题较为复杂，如果不采取剪枝策略容易超时，所以需要采取剪枝策略。另外，需要注意的是如果采取传统的回溯也会产生重复解的问题，所以也需要在回溯过程中进行处理。

在解决本问题的初期我选取了一定的剪枝+哈希表的方法来做，但复杂度仍较高（36ms）, 最后采用了不同策略解决本问题(2ms)。在下面代码中将结合注释指出解决本问题的细节。

**代码**

```java
    class Solution {
        public List<List<Integer>> combinationSum2(int[] candidates, int target) {
            //如果不采取去重策略，应该用哈希表去存储每次回溯完成的路径
            List<List<Integer>> res = new ArrayList<>();
            List<Integer> path = new ArrayList<>();
            //在回溯之前应该先对数组排序，方便回溯
            Arrays.sort(candidates);
            dfs(res,path,candidates,target,0);
            return res;
        }
        public void dfs(List<List<Integer>> res,List<Integer> path,int[] candidates,int target,int depth){
            if(target < 0) return;
            if(target == 0){
                res.add(new ArrayList<>(path));
                return;
            }
            for(int i = depth;i < candidates.length;i++){
               //很重要的剪枝（去重）操作判断i和depth以及candidates[i]和candidates[i - 1]的关系来去重
                if (i > depth && candidates[i] == candidates[i - 1]) {
                    continue;
                }
              //由于数组已经是有序的，所以可以剪枝
                if(target<candidates[i]) break;
                if(target>=candidates[i]){
                    path.add(candidates[i]);
                    dfs(res,path,candidates,target-candidates[i],i+1);
                    path.remove(path.size()-1);
                }
            }
        }
    }
```



## 总结

回溯问题可以通过剪枝操作来降低时间复杂度，但要注意在剪枝之前需要进行一些预处理。另外在回溯过程中的去重操作尽量不要选取哈希表进行去重，这样时间复杂度较高。
