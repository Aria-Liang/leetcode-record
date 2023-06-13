# Day34 Greedy Algorithm 03

## Related Questions:
### [1005. Maximize Sum Of Array After K Negations](https://leetcode.com/problems/maximize-sum-of-array-after-k-negations/description/)
```
class Solution {
    public int largestSumAfterKNegations(int[] nums, int k) {
        Arrays.sort(nums);
        int index = 0;
        while (k > 0  && index < nums.length && nums[index] < 0){
            System.out.println(nums[index]);
            nums[index] = -nums[index];
            k -= 1;
            index += 1;
        }  

        System.out.println(k); 
        
        if (k % 2 == 0 || (index < nums.length && nums[index] == 0)){
            return sum(nums);
        } 
        else{
            if (index > 0 && index <= nums.length - 1){
                int pre = nums[index - 1];
                int curr = nums[index];
                return sum(nums) - 2* Math.min(pre,curr);
            }
            else if (index == 0){
                return sum(nums) - 2*nums[index];
            }
            else{
                return sum(nums) - 2*nums[index - 1];
            }
        }
    }

    private int sum(int[] nums){
        int outcome = 0;
        for (int i = 0; i < nums.length; i++){
            outcome += nums[i];
        }
        return outcome;
    }
}
```
- 思路比较容易想：局部最优：把绝对值大的负数变成整数，整体最优：整个数组和最大
- 我的思路是直接sort,这样的话需要讨论左边的绝对值大还是右边，不够好，如果直接按照绝对值排序会比较简单
--------------------------------------------------
- 如何按照绝对值排序： 
```
nums = IntStream.of(nums)
		     .boxed()
		     .sorted((o1, o2) -> Math.abs(o2) - Math.abs(o1))
		     .mapToInt(Integer::intValue).toArray();
```
- Array can be converted with IntStream, 具体方法是： 
- Array -> IntStream: IntStream.of(nums)
- IntStream -> Array: .toArray()
------------------------------------------------
- Arrays can also be converted to Stream object, 具体方法是：
- Arrays.stream(nums)
- 所以  IntStream.of(nums).boxed() == Arrays.stream(nums)
------------------------------------------------
- Stream object  change to IntStream: 
- mapToInt(Integer::intvalue) maps the elements in the stream to their 'int' value
-----------------------------------------------------
- 变成Stream object之后会有一系列operation:
```
int sum = Arrays.stream(numbers)
                .filter(n -> n % 2 == 0)
                .sum();
```
完整写法：
```
class Solution {
    public int largestSumAfterKNegations(int[] nums, int k) {
        nums = IntStream.of(nums)
                        .boxed()
                        .sorted((o1, o2)-> Math.abs(o2) - Math.abs(o1))
                        .mapToInt(Integer::intValue).toArray();
        
        for (int i = 0; i < nums.length; i++){
            if (k > 0 && nums[i] < 0){
                nums[i] = - nums[i];
                k -= 1;
            }
        }

        if (k % 2 == 1){
            nums[nums.length - 1] = - nums[nums.length - 1];
        }

        return Arrays.stream(nums).sum();
    }
}
```

### [134. Gas Station](https://leetcode.com/problems/gas-station/description/)
- 这题贪心算法好难想，主要思维是这样的：
- 已知gas[], cost[], 自然我们就会想到res = gas - cost, 按照正常思路我们会想到从一个正数出发，如果走一圈最小值不为负那么就是那从那个点出发就好了，这种写法需要O(n^2)
- 但是有更好的解法，如果我们从index = 0开始走，一路相加prefixsum, 一旦sum < 0那么说明从前面任意一个出发都不合适，因为如果存在合适的index = i, 那么 0 - i 这一段的prefixsum肯定为负， 已经被我们处理过了
- 第二个难点是如果我从下一个点出发，那我还要不要遍历到最后一个点之后再去遍历前面的点，不用：
- 一个原因是我们会同时遍历totalsum,来判断是否存在这么一点点，如果totalsum < 0自然是不存在，但是如果totalsum > 0必定存在一个点
- 把prefixsum为负的所有距离看成一个整体，后面prefixSum为正的看成一个个合在一起的小正数整体，那么只要prefixsum>0，对于后一个数而言，加上总比不加好 
```
class Solution {
    public int canCompleteCircuit(int[] gas, int[] cost) {
        int startindex = 0;
        int sum = 0;
        int presum = 0;

        for (int i = 0; i< gas.length; i++){
            int res = gas[i] - cost[i];
            presum += res;
            sum += res;

            if (presum < 0){
                startindex = i + 1;
                presum = 0;
            }
        }

        if (sum < 0){return - 1;}
        else{return startindex;}
    }
}
```

### [135. Candy](https://leetcode.com/problems/candy/description/)
这题好巧妙哦，先处理右边比左边大的情况，再处理左边比右边大的情况。
```
class Solution {
    public int candy(int[] ratings) {
        int[] process = new int[ratings.length];
        process[0] = 1;
        int min = Integer.MAX_VALUE;

        for (int i = 1; i < ratings.length; i++){
            if (ratings[i] > ratings[i - 1]){
                process[i] = process[i - 1] + 1;
            }
            else{
                process[i] = 1;
            }
        }

        
        for (int i = ratings.length - 2; i >= 0; i--){
            if (ratings[i] > ratings[i + 1]){
                process[i] = Math.max(process[i], process[i + 1] + 1);
            }
        }

        int sum = 0;
        for (Integer num: process){
            sum += num;
        }

        return sum;
    }
}
```
