# Day31  Greedy Algorithm01

## Related Questions:
### [455. Assign Cookies](https://leetcode.com/problems/assign-cookies/description/)
```
class Solution {
    public int findContentChildren(int[] g, int[] s) {
        Arrays.sort(s);
        Arrays.sort(g);

        int child = 0;
        int cookie = 0;

        while (cookie < s.length && child < g.length){
            while (cookie < s.length && s[cookie] < g[child]){
                cookie++;
            }
            if (cookie < s.length && s[cookie] >= g[child]){
                child++;
                cookie++;
            }  
        }

        return child;
    }
}
```
思维基本一致，只是这里既可以优先满足小胃口的也可以优先满足大胃口的
```
class Solution {
    // 思路1：优先考虑饼干，小饼干先喂饱小胃口
    public int findContentChildren(int[] g, int[] s) {
        Arrays.sort(g);
        Arrays.sort(s);
        int start = 0;
        int count = 0;
        for (int i = 0; i < s.length && start < g.length; i++) {
            if (s[i] >= g[start]) {
                start++;
                count++;
            }
        }
        return count;
    }
}
class Solution {
    // 思路2：优先考虑胃口，先喂饱大胃口
    public int findContentChildren(int[] g, int[] s) {
        Arrays.sort(g);
        Arrays.sort(s);
        int count = 0;
        int start = s.length - 1;
        // 遍历胃口
        for (int index = g.length - 1; index >= 0; index--) {
            if(start >= 0 && g[index] <= s[start]) {
                start--;
                count++;
            }
        }
        return count;
    }
}
```

### [376. Wiggle Subsequence](https://leetcode.com/problems/wiggle-subsequence/)
```
class Solution {
    public int wiggleMaxLength(int[] nums) {
        boolean increase = true;
        int removenum = 0;
        if (nums.length <= 1){
            return nums.length;
        }

        int index = 1;

        while (index < nums.length && nums[index] == nums[index - 1]){
            index++;
            removenum++;     
        }

        if (index < nums.length && nums[index] - nums[0] < 0){
            increase = !increase;
        }

        
        while (index < nums.length){
            if (increase == true){
                int last = nums[index - 1];
                while (index < nums.length && nums[index] <= last){
                    removenum += 1;
                    index += 1;
                }
                index += 1;
                increase = !increase;
            }
            else{
                int last = nums[index - 1];
                while (index < nums.length && nums[index] >= last){
                    removenum += 1;
                    index += 1;
                }
                index += 1;
                increase = !increase;
            }
        }

        return nums.length - removenum;
    }
}
```
思路不太对，我的思路是比如上下上上上下，我就记录第三个上，因为第四个上出了问题，应该是下才对，那么我找第一个数值比三个上要小的，但这里可能会出问题，因为应该找上上上中最大的那个上
如图：
![image](https://github.com/Aria-Liang/leetcode-record/assets/97623323/a1c875dc-9aa0-41da-9cef-d9ea165f4658)
- 局部最优：删除单调坡度上的节点（不包括单调坡度两端的节点），那么这个坡度就可以有两个局部峰值。
- 整体最优：整个序列有最多的局部峰值，从而达到最长摆动序列
```
class Solution {
    public int wiggleMaxLength(int[] nums) {
        if (nums.length <= 1){
            return nums.length;
        }
        int peak = 1;
        int prediff = 0;
        int currdiff = 0;
        for (int i = 1; i < nums.length; i++){
            currdiff = nums[i] - nums[i - 1];
            if (currdiff > 0 && prediff <= 0 || currdiff < 0 && prediff >= 0){
                peak += 1;
                prediff = currdiff;
            }
        }
        return peak;
    }
}
```
数峰值就可以

### [53. Maximum Subarray](https://leetcode.com/problems/maximum-subarray/description/)
```
class Solution {
    public int maxSubArray(int[] nums) {
        if (nums.length == 0){
            return 0;
        }
        
        int minsum = 0;
        int totalsum = 0;
        int max = 0;

        for (int i = 0; i < nums.length; i++){
            totalsum += nums[i];

            if (i == 0){
                max = totalsum;
            }
            else{
                max = Math.max(max, totalsum - minsum);
            }
            minsum = Math.min(minsum, totalsum);
        }

        return max;
    }
}
```
优化版

```
class Solution {
    public int maxSubArray(int[] nums) {
        if (nums.length == 0){
            return 0;
        }
        
        int minsum = 0;
        int totalsum = 0;
        int max = Integer.MIN_VALUE;

        for (int i = 0; i < nums.length; i++){
            totalsum += nums[i];

            
            max = Math.max(max, totalsum - minsum);
            
            minsum = Math.min(minsum, totalsum);
        }

        return max;
    }
}
```
运用贪心算法的思路：当前面的totalsum为正的时候，始终是加上前面的数字会是sum更大的subarray,但是当前面的totalsum为负的时候，不加会是更好的
```
class Solution {
    public int maxSubArray(int[] nums) {
        int totalsum = 0;
        int maxsum = Integer.MIN_VALUE;

        for (int i = 0; i < nums.length; i++){
            totalsum += nums[i];
            maxsum = Math.max(maxsum, totalsum);

            if (totalsum < 0){
                totalsum = 0;
            }
        }

        return maxsum;
    }
}
```
