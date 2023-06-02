# Day24 BackTrack 01

## Use case:
### 回溯法，一般可以解决如下几种问题：
- 组合问题：N个数里面按一定规则找出k个数的集合
- 切割问题：一个字符串按一定规则有几种切割方式
- 子集问题：一个N个数的集合里有多少符合条件的子集
- 排列问题：N个数按一定规则全排列，有几种排列方式
- 棋盘问题：N皇后，解数独等等

##  Basic Structure
```
void backtracking(参数) {
    if (终止条件) {
        存放结果;
        return;
    }

    for (选择：本层集合中元素（树中节点孩子的数量就是集合的大小）) {
        处理节点;
        backtracking(路径，选择列表); // 递归
        回溯，撤销处理结果
    }
}

```


## Related Questions:
### [77. Combinations](https://leetcode.com/problems/combinations/description/)
```
class Solution {
    public List<List<Integer>> combine(int n, int k) {
        List<List<Integer>> result = new ArrayList<>();
        List<Integer> temp = new ArrayList<>();
        traceback(result, 1, n, k, temp);
        return result;
    }

    private void traceback(List<List<Integer>> result, int start, int n, int k, List<Integer> temp){
        if (k == 0){
            result.add(new ArrayList<>(temp));
            return;
        }

        for (int i = start; i <= n; i++){
            temp.add(i);
            traceback(result, i + 1, n, k - 1, temp);
            temp.remove(temp.size() - 1);
        }
    }
}
```
有两个地方可以优化：
1. 可以修剪树枝
![image](https://github.com/Aria-Liang/leetcode-record/assets/97623323/74d78740-437f-4eab-a42a-8fdbd67ad545)
2. 可以把一些变量变成全局变量
```
class Solution {
    List<List<Integer>> result = new ArrayList<>();
    List<Integer> temp = new ArrayList<>();
    public List<List<Integer>> combine(int n, int k) {
        traceback(1, n, k);
        return result;
    }

    private void traceback(int start, int n, int k){
        if (temp.size() == k){
            result.add(new ArrayList<>(temp));
            return;
        }

        for (int i = start; i <= n - k + temp.size() + 1; i++){
            temp.add(i);
            traceback(i + 1, n, k);
            temp.remove(temp.size() - 1);
        }
    }
}
```
