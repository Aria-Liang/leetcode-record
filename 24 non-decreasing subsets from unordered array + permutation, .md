# Day29 TrackBack 05

## Related Questions:
### [491. Non-decreasing Subsequences](https://leetcode.com/problems/non-decreasing-subsequences/)
错误解法：这题两个要求，第一原来的int[] nums没有排序，第二要求得到的subarray要non-decreasing order
我的解法只解决了能让subarray non-decreasing order, 但是由于int[] nums本身无序，无法用简单的nums[i] == nums[i - 1]去重来解决
```
class Solution {
    List<List<Integer>> result = new ArrayList<>();
    List<Integer> temp = new ArrayList<>();   
      
    public List<List<Integer>> findSubsequences(int[] nums) {
        // Arrays.sort(nums);
        backTrack(nums, 0);
        return result;
    }

    private void backTrack(int[] nums, int start){
        if (temp.size() >= 2){
            result.add(new ArrayList<>(temp));
        }

        for (int i = start; i < nums.length; i++){
            if (i != start && nums[i] == nums[i - 1]){
                continue;
            }

            else if (temp.size() >= 1 && nums[i] < temp.get(temp.size() - 1)){
                continue;
            }
            else{
                temp.add(nums[i]);
                backTrack(nums, i + 1);
                temp.remove(temp.size() - 1);
            }
            
        }
    }
}
```
正确解法：同样也是层级去重，所以每层需要一个用来标记是否使用过
```
class Solution {
    List<List<Integer>> result = new ArrayList<>();
    List<Integer> temp = new ArrayList<>();   
      
    public List<List<Integer>> findSubsequences(int[] nums) {
        // Arrays.sort(nums);
        backTrack(nums, 0);
        return result;
    }

    private void backTrack(int[] nums, int start){
        if (temp.size() >= 2){
            result.add(new ArrayList<>(temp));
        }
        
        int[] used = new int[201];                 //因为数字范围是-100 到 100

        for (int i = start; i < nums.length; i++){
            if (used[nums[i] + 100] == 1){
                continue;
            }

            else if (temp.size() >= 1 && nums[i] < temp.get(temp.size() - 1)){
                continue;
            }
            else{
                used[nums[i] + 100] = 1;
                temp.add(nums[i]);
                backTrack(nums, i + 1);
                temp.remove(temp.size() - 1);
            }
            
        }
    }
}
```

### [46. Permutations](https://leetcode.com/problems/permutations/description/)
```
class Solution {
    List<List<Integer>> result = new ArrayList<>();
    List<Integer> temp = new ArrayList<>();

    public List<List<Integer>> permute(int[] nums) {
        backTrack(nums);
        return result;
    }

    private void backTrack(int[] nums){
        if (temp.size() == nums.length){
            result.add(new ArrayList<>(temp));
            return;
        }

        for (int i = 0; i < nums.length; i++){
            if (temp.contains(nums[i])){
                continue;
            }
            temp.add(nums[i]);
            backTrack(nums);
            temp.remove(temp.size() - 1);
        }
    }
}
```
最好不用contains，因为time complexity 是 O(n), 可以用int[] used
```
class Solution {

    List<List<Integer>> result = new ArrayList<>();// 存放符合条件结果的集合
    LinkedList<Integer> path = new LinkedList<>();// 用来存放符合条件结果
    boolean[] used;
    public List<List<Integer>> permute(int[] nums) {
        if (nums.length == 0){
            return result;
        }
        used = new boolean[nums.length];
        permuteHelper(nums);
        return result;
    }

    private void permuteHelper(int[] nums){
        if (path.size() == nums.length){
            result.add(new ArrayList<>(path));
            return;
        }
        for (int i = 0; i < nums.length; i++){
            if (used[i]){
                continue;
            }
            used[i] = true;
            path.add(nums[i]);
            permuteHelper(nums);
            path.removeLast();
            used[i] = false;
        }
    }
}
```

### [47. Permutations II](https://leetcode.com/problems/permutations-ii/description/)
```
class Solution {
    List<List<Integer>> result = new ArrayList<>();
    List<Integer> temp = new ArrayList<>();
    List<Integer> index = new ArrayList<>();

    public List<List<Integer>> permuteUnique(int[] nums) {
        backTrack(nums);
        return result;
    }

    private void backTrack(int[] nums){
        if (temp.size() == nums.length){
            result.add(new ArrayList<>(temp));
            return;
        }
        
        int[] used = new int[21];
        for (int i = 0; i < nums.length; i++){
            if (index.contains(i)){
                continue;
            }

            if (used[nums[i] + 10] == 1){
                continue;
            }

            used[nums[i] + 10] = 1;
            index.add(i);
            temp.add(nums[i]);
            backTrack(nums);
            index.remove(index.size() - 1);
            temp.remove(temp.size() - 1);
        }
    }
}
```

可以先进行排序，因为这是一个排列问题，所以原本的顺序不重要
```
class Solution {
    List<List<Integer>> result = new ArrayList<>();
    List<Integer> temp = new ArrayList<>();
    //List<Integer> index = new ArrayList<>();

    public List<List<Integer>> permuteUnique(int[] nums) {
        int[] used = new int[nums.length];
        Arrays.sort(nums);
        backTrack(nums, used);
        return result;
    }

    private void backTrack(int[] nums, int[] used){
        if (temp.size() == nums.length){
            result.add(new ArrayList<>(temp));
            return;
        }
        
        for (int i = 0; i < nums.length; i++){
            if (i > 0 && nums[i] == nums[i - 1] && used[i - 1] == 1){     //同层避免重复，之所以需要used[i - 1] == 1， 也可以used[i - 1] == 0
                continue;
            }

            if (used[i] == 1){
                continue;
            }

            used[i] = 1;
            //index.add(i);
            temp.add(nums[i]);
            backTrack(nums, used);
            //index.remove(index.size() - 1);
            temp.remove(temp.size() - 1);
            used[i] = 0;
        }
    }
}
```

used[i - 1] == 0, 相当于选代表选第一个， 层级去重
![image](https://github.com/Aria-Liang/leetcode-record/assets/97623323/7308764f-2652-42ba-aa82-dd2f869c3ce9)
同一层如果前一个没选，我就不选了

used[i - 1] == 1, 我个人的理解是相当于选代表选了最后一个，要从后往前
![image](https://github.com/Aria-Liang/leetcode-record/assets/97623323/4b8a1483-a94d-43e3-bcc9-a0c15b956293)
但不算特别理解，之后再思考
