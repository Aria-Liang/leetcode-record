# Day7 HashTable 02

## Related Questions:
### [15. 3Sum](https://leetcode.com/problems/3sum/description/)
选定一个点，另外两个点做two sum, 这题一开始比较让我纠结的点是为什么选完点之后，另外两个点一个是i+1，另一个是nums.lenth - 1
其实使我们人为assume a <= b <= c, 我们遍历的其实是a

另外易错点是，不能出现同样的两组元素，所以要选代表
```
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        //假设a <= b <= c, a+b+c = 0, b+c = -a
        List<List<Integer>> result = new ArrayList<>();

        if (nums.length == 0){
            return result;
        }

        Arrays.sort(nums);

        for (int i = 0; i < nums.length - 2;; i++){
            if (i > 0 && nums[i] == nums[i - 1]){                            //选代表
                continue;
            }

            int left = i + 1;
            int right = nums.length - 1;
            int target = -nums[i];

            List<List<Integer>> output = TwoSum(nums, left, right, target);

            for (List<Integer> ele: output){                              
                List<Integer> re = new ArrayList<>();
                re.add(nums[i]);
                for (Integer num: ele){
                    re.add(num);
                }
                result.add(re);
            }
        }
        return result;
    }

    private List<List<Integer>> TwoSum(int[] nums, int left, int right, int target){
        List<List<Integer>> output = new ArrayList<>();

        while (left < right){
            if (nums[left] + nums[right] == target){
                List<Integer> out = new ArrayList<>();
                out.add(nums[left]);
                out.add(nums[right]);
                output.add(out);
                left++;
                right--; 
                while (left < right && nums[left] == nums[left - 1]){      // 选代表
                    left++;
                }
                while (left < right && nums[right] == nums[right + 1]){    // 选代表
                    right--;
                }
            }
            else if (nums[left] + nums[right] < target){                 //这里不需要，如果不相等，即使一样也是不会被选中
                left++;
            }
            else if (nums[left] + nums[right] > target){                //这里不需要，如果不相等，即使一样也是不会被选中
                right--;
            }
        }

        return output;
    }
}
```
有两个可以优化的地方， 1.可以写成 result.add(Arrays.asList(nums[i], nums[left], nums[right]));
                     2. 可以直接把result传进twosum里面，直接加进去
                     
                     
###[18. 4Sum](https://leetcode.com/problems/4sum/description/)
和3sum一样，只是这次是固定a,b两个点，twosum c,d两个点
```
class Solution {
    public List<List<Integer>> fourSum(int[] nums, int target) {
        List<List<Integer>> result = new ArrayList<>();

        // a <= b <= c <= d   a+b+c+d = target  c+d = target -a -b
        Arrays.sort(nums);

        for (int i = 0; i < nums.length - 3; i++){
            if (nums[i] > 0 && nums[i] > target) {
                return result;
            }

            if (i > 0 && nums[i] == nums[i - 1]){
                continue;
            }

            for (int j = i + 1; j < nums.length - 2; j++){
                if (j > i + 1 && nums[j] == nums[j - 1]){
                    continue;
                }

                int left = j + 1;
                int right = nums.length - 1;
                long sum = (long)target -(nums[i] + nums[j]);    //数据范围可能很
                int first = i;
                int second = j;

                TwoSum(nums, left, right, sum, first, second, result); 
            }
        }

        return result;
    }

    private void TwoSum(int[] nums, int left, int right, long sum, int firstIndex, int SecondIndex, List<List<Integer>> result){
        while (left < right){
            if (nums[left] + nums[right] == sum){
                result.add(Arrays.asList(nums[firstIndex], nums[SecondIndex], nums[left], nums[right]));
                left++;
                right--;
                while (left < right && nums[left] == nums[left - 1]){
                    left++;
                }
                while (left < right && nums[right] == nums[right + 1]){
                    right--;
                }
            }
            else if (nums[left] + nums[right] < sum){
                left++;
            }
            else if (nums[left] + nums[right] > sum){
                right--;
            }   
        }
    }
}
```

###[454. 4Sum II](https://leetcode.com/problems/4sum-ii/description/)
```
class Solution {
    public int fourSumCount(int[] nums1, int[] nums2, int[] nums3, int[] nums4) {
        HashMap<Integer, Integer> map = new HashMap<>();

        for (int i = 0; i < nums1.length; i++){
            int first = nums1[i];
            for (int j = 0; j < nums2.length; j++){
                int second = nums2[j];
                int sum = first + second;
                if (map.containsKey(sum)){
                    int val = map.get(sum);
                    map.put(sum, val + 1);
                }
                else{
                    map.put(sum, 1);
                }
            }
        }

        int totalNum = 0;
        for (int k = 0; k < nums3.length; k++){
            int third = nums3[k];
            for (int l = 0; l < nums4.length; l++){
                int forth = nums4[l];
                int sum2 = third + forth;
                if (map.containsKey(-sum2)){
                    totalNum += map.get(-sum2);
                }
            }
        }

        return totalNum;
    }
}
```
