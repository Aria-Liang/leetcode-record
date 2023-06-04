# Day27 TraceBack 03

## Related Questions:
### [40. Combination Sum II](https://leetcode.com/problems/combination-sum-ii/)
```
class Solution {
    List<List<Integer>> result = new ArrayList<>();
    List<Integer> temp = new ArrayList<>();
    
    public List<List<Integer>> combinationSum2(int[] candidates, int target) {
        Arrays.sort(candidates);
        traceback(candidates, 0, target);
        return result;
    }

    private void traceback(int[] candidates, int start, int target){
        if (target == 0){
            result.add(new ArrayList<>(temp));
            return;
        }

        if (target < 0){
            return;
        }

        for (int i = start; i < candidates.length; i++){
            if (i != start && candidates[i] == candidates[i - 1]){
                continue;
            }
            temp.add(candidates[i]);
            traceback(candidates, i + 1, target - candidates[i]);
            temp.remove(temp.size() - 1);
        }
    }
}
```

### [39. Combination Sum](https://leetcode.com/problems/combination-sum/)
```
class Solution {
    List<List<Integer>> result = new ArrayList<>();
    List<Integer> temp = new ArrayList<>();
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        Arrays.sort(candidates);
        traceback(candidates, target, 0);
        return result;
    }

    private void traceback(int[] candidates, int target, int start){
        if (target < 0){
            return;
        }

        if (target == 0){
            result.add(new ArrayList<>(temp));
            return;
        }

        for (int i = start; i < candidates.length; i++){
            if (i > 0 && candidates[i] == candidates[i - 1]){
                continue;
            }
            temp.add(candidates[i]);
            traceback(candidates, target - candidates[i], i);
            temp.remove(temp.size() - 1);
        }
    }
}
```

### [131. Palindrome Partitioning](https://leetcode.com/problems/palindrome-partitioning/)
```
class Solution {
    List<List<String>> result = new ArrayList<>();
    List<String> temp = new ArrayList<>();
    public List<List<String>> partition(String s) {
        if (s.length() == 0){
            return result;
        }
        traceback(s, 0);
        return result;

    }

    private void traceback(String s, int start){
        if (start == s.length()){
            result.add(new ArrayList<>(temp));
            return;
        }

        for (int i = start; i < s.length(); i++){
            String sb = s.substring(start, i + 1);
            if (!isPalindrome(sb)){
                continue;
            }
            temp.add(sb);
            traceback(s, i+1);
            temp.remove(temp.size() - 1);
        }
    }

    private boolean isPalindrome(String s){
        int start = 0; 
        int end = s.length() - 1;
        while (start < end){
            if (s.charAt(start) != s.charAt(end)){
                return false;
            }
            start += 1;
            end -= 1;
        }
        return true;
    }
}
```
