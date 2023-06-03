# Day25 TraceBack 02

## Related Questions:
### [216. Combination Sum III](https://leetcode.com/problems/combination-sum-iii/description/)
```
class Solution {
    List<List<Integer>> result = new ArrayList<>();
    List<Integer> temp = new ArrayList<>();
    int tempSum = 0;
    public List<List<Integer>> combinationSum3(int k, int n) {
        traceBack(k, n, 1);
        return result;
    }

    private void traceBack(int k, int n, int start){
        if (temp.size() == k){
            if (tempSum == n){
                result.add(new ArrayList<>(temp));
            }
            return;
        }

        for (int i = start; i <= 9 - k + temp.size() + 1; i++){
            tempSum += i;
            temp.add(i);
            traceBack(k, n, i+1);
            tempSum -= i;
            temp.remove(temp.size() - 1);
        }
    }
}
```
基本正确，剪枝操作少了一步，如果sum > targetSum, 可以直接返回
```
class Solution {
	List<List<Integer>> result = new ArrayList<>();
	LinkedList<Integer> path = new LinkedList<>();

	public List<List<Integer>> combinationSum3(int k, int n) {
		backTracking(n, k, 1, 0);
		return result;
	}

	private void backTracking(int targetSum, int k, int startIndex, int sum) {
		// 减枝
		if (sum > targetSum) {
			return;
		}

		if (path.size() == k) {
			if (sum == targetSum) result.add(new ArrayList<>(path));
			return;
		}

		// 减枝 9 - (k - path.size()) + 1
		for (int i = startIndex; i <= 9 - (k - path.size()) + 1; i++) {
			path.add(i);
			sum += i;
			backTracking(targetSum, k, i + 1, sum);
			//回溯
			path.removeLast();
			//回溯
			sum -= i;
		}
	}
}
```

### [17. Letter Combinations of a Phone Number](https://leetcode.com/problems/letter-combinations-of-a-phone-number/)
```
class Solution {
    List<String> result = new ArrayList<>();
    public List<String> letterCombinations(String digits) {
        Map<Character, String[]> map = new HashMap<>();
        mapping(map);
        int num = digits.length();
        
        List<String[]> list = new ArrayList<>();
        for (int i = 0; i < digits.length(); i++){
            char element = digits.charAt(i);
            String[] value = map.get(element);
            list.add(value);
            //System.out.println(value);
            //start[i] = value.length;
        }

        StringBuilder sb = new StringBuilder();
        traceback(sb, 0, list, num);
        return result;

    }

    private void traceback(StringBuilder sb, int level, List<String[]> list, int num){
        if (sb.length() == num){
            result.add(sb.toString());
            return;
        }

        String[] value = list.get(level);
        for (int i = 0; i < value.length; i++){
            sb.append(value[i]);
            traceback(sb, level + 1, list, num);
            int lastIndex = sb.length() - 1;
            sb.deleteCharAt(lastIndex);
            level-=1;
        }
    }

    private void mapping(Map<Character, String[]> map){
        String[][] array = {
            {"a", "b", "c"},
            {"d", "e", "f"},
            {"g", "h", "i"},
            {"j", "k", "l"},
            {"m", "n", "o"},
            {"p", "q", "r", "s"},
            {"t", "u", "v"},
            {"w", "x", "y", "z"}
        };

        for (int i = 2; i <= 9; i++){
            map.put((char)i, array[i-2]);
        }
    }
}
```
感觉思路基本正确，但是不知道哪里出了问题，代码跑不通，这里其实不需要创建map,一个String[]就可以
```
class Solution {
    List<String> result = new ArrayList<>();
    StringBuilder sb = new StringBuilder();
    public List<String> letterCombinations(String digits) {
        if (digits.length() == 0){
            return result;
        }

        String[] numString = {"", "", "abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"};

        traceback(0, numString, digits);
        return result;

    }

    private void traceback(int level, String[] numString, String digits){
        if (sb.length() == digits.length()){
            result.add(sb.toString());
            return;
        }

        String value = numString[digits.charAt(level) - '0'];

        for (int i = 0; i < value.length(); i++){
            sb.append(value.charAt(i));
            traceback(level + 1, numString, digits);
            int lastIndex = sb.length() - 1;
            sb.deleteCharAt(lastIndex);
        }
    }
}
```
