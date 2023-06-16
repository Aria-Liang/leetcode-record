# Day38 Dynamic Programming 01
- 确定dp数组（dp table）以及下标的含义
- 确定递推公式
- dp数组如何初始化
- 确定遍历顺序
- 举例推导dp数组

## Related Questions:
### [509. Fibonacci Number](https://leetcode.com/problems/fibonacci-number/description/)
```
class Solution {
    public int fib(int n) {
        if (n == 0){
            return 0;
        }

        if (n == 1){
            return 1;
        }

        return fib(n - 1) + fib(n - 2);
    }
}
```
这是递归的做法，那么动态规划怎么做呢？
```
class Solution {
    public int fib(int n) {
        if (n <= 1) return n;             
        int[] dp = new int[n + 1]； //1.dp数组以及下标含义：dp[i]意思是第i个fib数值
        dp[0] = 0;             //3. dp数值初始化
        dp[1] = 1;
        for (int index = 2; index <= n; index++){  //4.确定遍历顺序
            dp[index] = dp[index - 1] + dp[index - 2];  2.确定递推公式
        }
        return dp[n];
    }
}
```

### [70. Climbing Stairs](https://leetcode.com/problems/climbing-stairs/)
```
class Solution {
    public int climbStairs(int n) {
        if (n <= 1){
            return 1;
        }
        
        int[] dp = new int[n];
        dp[0] = 1;
        dp[1] = 2;
        for (int i = 2; i < n; i++){
            dp[i] = dp[i - 1] + dp[i - 2];
        }
        return dp[n - 1];
    }
}
```
也是同样的思路
- 确定dp[i]意思是第i个台阶有多少种走法
n = 1      dp[0] = 1
n = 2      dp[1] = 2
n = 3      dp[3] = dp[1] + dp[2]因为往后退一步是dp[2],往后退两步是dp[1]

### [746. Min Cost Climbing Stairs](https://leetcode.com/problems/min-cost-climbing-stairs/description/)
```
class Solution {
    public int minCostClimbingStairs(int[] cost) {
        int[] dp = new int[cost.length];
        dp[0] = cost[0];
        dp[1] = cost[1];
        for (int i = 2; i < cost.length; i++){
            dp[i] = Math.min(dp[i - 1], dp[i - 2]) + cost[i];
        }
        return Math.min(dp[cost.length - 1], dp[cost.length - 2]);
    }
}
```
