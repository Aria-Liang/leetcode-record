# Day32 Greedy Algorithm 02

## Related Questions:
### [122. Best Time to Buy and Sell Stock II](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/description/)
```
class Solution {
    public int maxProfit(int[] prices) {
        if (prices.length <= 1){
            return 0;
        }

        int[] profit = new int[prices.length - 1];
        for (int i = 0; i < prices.length - 1; i++){
            profit[i] = prices[i + 1] - prices[i];
        }

        int totalsum = 0;
        for (Integer p : profit){
            if (p > 0){
                totalsum += p;
            }
        }

        return totalsum;
    }
}
```
- 因为利润是可以分解的
- 假如第0天买入，第3天卖出，那么利润为prices[3] - prices[0],可以分解为：(prices[3]-prices[2]) + (prices[2) - prices[1]) + (prices[1] - prices[0])
![image](https://github.com/Aria-Liang/leetcode-record/assets/97623323/1e5c088c-3391-48fe-b696-6e71d163097b)

可以简写为：
```
class Solution {
    public int maxProfit(int[] prices) {
        int result = 0;
        for (int i = 1; i < prices.length; i++) {
            result += Math.max(prices[i] - prices[i - 1], 0);
        }
        return result;
    }
}
```

### [55. Jump Game](https://leetcode.com/problems/jump-game/description/)
```
class Solution {
    public boolean canJump(int[] nums) {
        if (nums.length == 1){
            return true;
        }

        int last = nums.length - 1;
        for (int i = nums.length - 2; i >= 0; i--){
            if (nums[i] >= last - i){
                last = i;
            }
        }

        return last == 0;
    }
}
```
- 想复杂了，还有一种做法是贪心算法的思路：走到哪一步不重要，重要的是range
- 贪心算法局部最优解：每次取最大跳跃步数（取最大覆盖范围），整体最优解：最后得到整体最大覆盖范围，看是否能到终点
![image](https://github.com/Aria-Liang/leetcode-record/assets/97623323/20202a43-82a2-44b2-923b-7eaf56ec06f3)
这里要动态修改for循环的变量
```
class Solution {
    public boolean canJump(int[] nums) {
        int coverage = 0;
        for (int i = 0; i <= coverage; i++){
            coverage = Math.max(coverage, i + nums[i]);
            if (coverage >= nums.length - 1){
                return true;
            }
        }
        return false;
    }
}
```

### [45. Jump Game II](https://leetcode.com/problems/jump-game-ii/)
```
class Solution {
    public int jump(int[] nums) {
        int pre_coverage = 0;
        int curr_coverage = 0;
        int num = 0;
        int index = 0;
        while (index < nums.length){
            while (index <= pre_coverage && index < nums.length){
                curr_coverage = Math.max(curr_coverage, index + nums[index]);
                index ++;
            }
            if (pre_coverage >= nums.length - 1){
                break;
            }
            num += 1;
            pre_coverage = curr_coverage;
        }
        return num;
    }
}
```
优化版本
```
class Solution {
    public int jump(int[] nums) {
        int end = 0;
        int coverage = 0;
        int count = 0;

        for (int i = 0;  i <= end && end < nums.length - 1; i++){
            coverage = Math.max(coverage, i + nums[i]);
            if (i == end){
                count += 1;
                end = coverage;
            }
        }

        return count;
    }
}
```
