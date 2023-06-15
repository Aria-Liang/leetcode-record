# Day36 Greedy Algorithm 05

## Related Questions:
### [435. Non-overlapping Intervals](https://leetcode.com/problems/non-overlapping-intervals/description/)
```
class Solution {
    public int eraseOverlapIntervals(int[][] intervals) {
        if (intervals.length <= 1){
            return 0;
        }
        
        Arrays.sort(intervals, (a, b) -> {
            if (a[0] == b[0]) return a[1] - b[1];
            return a[0] - b[0];
        });

        int end = intervals[0][1];
        int count = 0;
        for (int i = 1; i < intervals.length; i++){
            if (intervals[i][0] < end){
                count += 1;
                end = Math.min(end, intervals[i][1]);
            }
            else{
                end = Math.max(end, intervals[i][1]);
            }
            
        }

        return count;
    }
}
```

### [763. Partition Labels](https://leetcode.com/problems/partition-labels/description/)
这题很有技巧性，"ababcbacadefegdehijhklij"， 有这么一个字符串，通过分隔使得同一个字母最多出现在一个片段中，那么我们首先应该找出每个字母第一个出现的位置和最后一个出现的位置，这样就变成了区间重叠的问题，但还有一种更简单的方法，只记录每个字母出现的位置，然后在遍历s中每一个character时，看看前面所有的字母出现的最后位置和当前的index是否相等，如果是就可以进行分隔
```
class Solution {
    public List<Integer> partitionLabels(String s) {
        int[] dic = new int[26];
        char[] charlist = s.toCharArray();
        for (int i = 0; i < s.length(); i++){
            int index = s.charAt(i) - 'a';
            dic[index] = i;
        }

        int lastindex = 0;
        int preindex = -1;
        List<Integer> result = new ArrayList<>();
        for (int i = 0; i < charlist.length; i++){
            lastindex = Math.max(lastindex, dic[charlist[i] - 'a']);

            if (lastindex == i){
                result.add(i - preindex);
                preindex = i;
            }
        }

        return result;
    }
}
```

### [56. Merge Intervals](https://leetcode.com/problems/merge-intervals/description/)
```
class Solution {
    public int[][] merge(int[][] intervals) {
        Arrays.sort(intervals, (a, b) -> {
            if (a[0] == b[0]) {return a[1] - b[1];}
            return a[0] - b[0];
        });

        List<int[]> list = new ArrayList<>();
        int start = intervals[0][0];
        int end = intervals[0][1];

        for (int i = 1; i < intervals.length; i++){
            if (intervals[i][0] <= end){
                end = Math.max(end, intervals[i][1]);
            }
            else{
                int[] arr = {start, end};
                list.add(arr);               //list(new int[]{start, end});
                start = intervals[i][0];
                end = intervals[i][1];
            }
        }

        int[] arr1 = {start, end};
        list.add(arr1);

        int[][] result = new int[list.size()][2];
        for (int i = 0; i < list.size(); i++) {
            result[i] = list.get(i);
        }                                         //list.toArray(new int[list.size()][]);

        return result;
    }
}
```
