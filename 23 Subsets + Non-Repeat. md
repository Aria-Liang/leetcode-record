# Day6 TrackBack 04

## Related Questions:
### [93. Restore IP Addresses](https://leetcode.com/problems/restore-ip-addresses/description/)
```
class Solution {
    List<String> result = new ArrayList<>();
    int num = 0;

    public List<String> restoreIpAddresses(String s) {
        traceback(s, 0, new String());
        return result;
    }

    private void traceback(String s, int start, String temp){
        if (start >= s.length()){
            return;
        }

        if (num == 3){
            String remain = s.substring(start, s.length());
            if (remain.length() > 4){
                return;
            }
            int re = Integer.parseInt(remain);
            System.out.println("temp" + temp);
            System.out.println("remian" + remain);
            if (re <= 255 && remain.equals(String.valueOf(re))){
                result.add(temp + "." + remain);
            }
            return;
        }

        for (int i = start; i < s.length(); i++){
            String tem = s.substring(start, i + 1);
            int te = Integer.parseInt(tem);
            //num += 1;
            if (te > 255 || !tem.equals(String.valueOf(te))){
                break;
            }
            
            num += 1;
            if (temp.length() == 0){
                traceback(s, i + 1, temp + tem);
                num -= 1;
            }
            else{
                traceback(s, i + 1, temp + "." + tem); 
                num -= 1;
            }
            
        }
    }
}
```
总结： 组合问题和分隔问题都是收集树的叶子结点，但是子集问题找的是树所有的节点
![image](https://github.com/Aria-Liang/leetcode-record/assets/97623323/a69df140-7c4e-4678-9a2c-9653ef9a43f1)

在子集问题中，模版的终止条件可以不要

### [78. Subsets](https://leetcode.com/problems/subsets/description/)
#### solution1, 思路是取或不取
```
class Solution {
    List<List<Integer>> result = new ArrayList<>();
    List<Integer> temp = new ArrayList<>();

    public List<List<Integer>> subsets(int[] nums) {
        if (nums.length == 0){
            return result;
        }
        backTrack(nums, 0);
        return result;
    }

    private void backTrack(int[] nums, int start){
        if (start == nums.length){
            result.add(new ArrayList<>(temp));
            return;
        }
 
        temp.add(nums[start]);
        backTrack(nums, start + 1);
        temp.remove(temp.size() - 1);
        backTrack(nums, start + 1);
        
    }
}
```
#### 思路是取完再放下，以及在for loop之前就把temp放入result中
```
class Solution {
    List<List<Integer>> result = new ArrayList<>();
    List<Integer> temp = new ArrayList<>();

    public List<List<Integer>> subsets(int[] nums) {
        if (nums.length == 0){
            return result;
        }
        backTrack(nums, 0);
        return result;
    }

    private void backTrack(int[] nums, int start){
        if (start > nums.length){  //可以不要
            return;
        }

        result.add(new ArrayList<>(temp));
        for (int i = start; i <nums.length; i++){
            temp.add(nums[i]);
            backTrack(nums, i + 1);
            temp.remove(temp.size() - 1);
        }
        
    }
}
```

去重问题，同样的套路，都是树层去重，树枝不去重
### [90. Subsets II](https://leetcode.com/problems/subsets-ii/description/)
```
class Solution {
    List<List<Integer>> result = new ArrayList<>();
    List<Integer> temp = new ArrayList<>();

    public List<List<Integer>> subsetsWithDup(int[] nums) {
        if (nums.length == 0){
            return result;
        }
        Arrays.sort(nums);
        backTrack(nums, 0);
        return result;
        
    }

    private void backTrack(int[] nums, int start){
        if (start > nums.length){            //可以不要
            return;
        }
        
        result.add(new ArrayList<>(temp));

        for (int i = start; i < nums.length; i++){
            if (i != start && nums[i] == nums[i - 1]){
                continue;
            }

            temp.add(nums[i]);
            backTrack(nums, i + 1);
            temp.remove(temp.size() - 1);
        }
    }
}
```
