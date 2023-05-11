# Day2 Array02

### two pointer
[977. Square of a sorted Array](https://leetcode.com/problems/squares-of-a-sorted-array/description/)
```
class Solution {
    public int[] sortedSquares(int[] nums) {
        if (nums.length == 0|| nums == null){
            return nums;
        }

        int left = 0;
        int right = nums.length - 1;
        int[] square = new int[nums.length];
        int index = nums.length - 1;

        while (left <= right){
            int left_square = nums[left]*nums[left];
            int right_square = nums[right]*nums[right];

            if (left_square < right_square){
                square[index] = right_square;
                right--;
            }
            else{
                square[index] = left_square;
                left++;
            }
            index--;
        }

        return square;
    }
}
```
一开始分了很多种情况，都大于0，都小于0， 或者有正有负，但其实没关系，双指针可以cover


#### Sliding windows
[209. Minimum Size Subarray Sum](https://leetcode.com/problems/minimum-size-subarray-sum/description/)
My answer
```
class Solution {
    public int minSubArrayLen(int target, int[] nums) {
        ArrayList<Integer> subarray = new ArrayList<Integer>();
        int sum = 0;
        int Min_size = 0;
        for (int i = 0; i < nums.length; i++){
            sum += ，nums[i];
            subarray.add(nums[i]);
            if (sum >= target){
                if (Min_size != 0){
                    Min_size = Math.min(subarray.size(), Min_size);
                }
                else{
                    Min_size = subarray.size();
                }
                while (sum >= target){
                    sum -= subarray.get(0);
                    subarray.remove(0);
                    if (sum >= target){
                        Min_size = Math.min(subarray.size(), Min_size);
                    }
                }
            }
        }
        return Min_size;
    }
}
```
代码可以简化，而且不需要另外创建一个ArrayList，直接记录一下left index即可
可以先明确思路：1.我们遍历的是right index， 从第一个数字遍历到最后一个数字，找包含right number的min-size
              2.当sum < target时，一直增加sum
              2. 当sum > target时，做两件事，一件事是记录一下长度，另一件事是一直删除第一个元素直到sum < target,这样才好再次进入循环，加上下一个number
```
class Solution {
    public int minSubArrayLen(int target, int[] nums) {
        if (nums.length == 0 || nums == null){
            return 0;
        }
        int left = 0;
        int sum = 0;
        int min_size = Integer.MAX_VALUE;

        for (int right = 0; right < nums.length; right++){
            sum += nums[right];
            while (sum >= target){
                min_size = Math.min(right - left + 1, min_size);
                sum -= nums[left];
                left++;
            }
        }

        return min_size == Integer.MAX_VALUE? 0: min_size;


    }
}
```

[904. Fruit Into Baskets](https://leetcode.com/problems/fruit-into-baskets/description/)
```
class Solution {
    public int totalFruit(int[] fruits) {
        if (fruits.length <= 2){
            return fruits.length;
        }

        int start = 0;
        int first_index = 0;
        int second_index = 1;
        while (second_index < fruits.length - 1 && fruits[second_index] == fruits[first_index]){
            first_index++;
            second_index++;
        }
        int max_size = Math.max(first_index, second_index) - start + 1;

        for (int i = second_index + 1; i < fruits.length; i++){
            if (fruits[i] == fruits[first_index]){
                first_index = i;
            }
            else if (fruits[i] == fruits[second_index]){
                second_index = i;
            }
            else{
                if (first_index < second_index){
                    start = first_index + 1;
                    first_index = i;
                }
                else{
                    start = second_index + 1;
                    second_index = i;
                }

            }
            max_size = Math.max(max_size, (Math.max(first_index, second_index) - start + 1));
        }
        return max_size;
    }
}
```
思路也是sliding windows, 本别记录first element和second element的最后一个元素，如果发现一个不属于这两个元素的，则把窗口收束到Math.min(first elemnt index, second element index);

本题错误点：
1.  while (second_index < fruits.length - 1 && fruits[second_index] == fruits[first_index])
是< fruits.length - 1， 如果写fruits.length 会out of range
2.  if (fruits.length <= 2), 有等于号，因为两个元素， 一个元素， 零个元素都输出fruits.length

[76. Minimum Window Substring](https://leetcode.com/problems/minimum-window-substring/description/) hard
没写完

###螺旋矩阵
[59. Spiral Matrix II](https://leetcode.com/problems/spiral-matrix-ii/description/)
```
class Solution {
    public int[][] generateMatrix(int n) {
        // n = 3 2, n = 4 2, n = 5 3, n = 6 3   n+1//2
        // num = 3^2 - 1^2 = 8 num = 4^2 - 2^2
        int round = n/2;
        int[][] result = new int[n][n];
        int startNum = 1;
        //round 0;
        //int num = n*n - (n - 2)*(n - 2);
        //round 1;
        //int num = (n-2*round)*(n-2*round) - (n-2*(round+1))*(n-2*(round+1));
        for (int i = 0; i < round; i++){
            int num = (n-2*i)*(n-2*i) - (n-2*(i+1))*(n-2*(i+1));
            fillelement(result, n, i, startNum);
            startNum += num;
        }
        
        if (n % 2 == 1){
            result[n/2][n/2] = startNum;
        }

        return result;
    }

    public void fillelement(int[][] result, int n, int round, int startNum){
        // n = 3, round = 0, start = 1
        //start point = (round, round) --- (round, n- round - 2)
        //(round, n - round - 2) --- (n - round - 2, n - round - 2)
        //(n-round-2, n-round-2) --- (round, n- round - 2)
        //(round, n - round - 2) --- (round, round + 1)
        for (int j = round; j <= n - round - 2; j++){ //j = 0 1
            result[round][j] = startNum;              //result[0][0] = 1 [0][1] = 2
            startNum++;                               //start = 3
        }
        for (int k = round; k <= n - round - 2; k++){ //k = 0 1
            result[k][n-round-1] = startNum;          //result[0][2] = 3; [1][2] = 4 
            startNum++;                               //start = 5
        }
        for (int l = n-round-1; l >= round + 1; l--){ //l = 2 1
            result[n-round-1][l] = startNum;          //result[2][2] = 5 [1][2] = 6
            startNum++;                               //start = 4
        }
        for (int m = n-round-1; m >= round + 1;m--){  //m = 1
            result[m][round] = startNum;              //result[1][0] = 4
            startNum++;
        }
    }
}
```
这题主要是要搞清楚怎么绕的

[54. Spiral Matrix](https://leetcode.com/problems/spiral-matrix/description/)
```
class Solution {
    public List<Integer> spiralOrder(int[][] matrix) {
        int m = matrix.length;   //2
        int n = matrix[0].length;//1

        List<Integer> result = new ArrayList<>();

        int round = Math.min(m,n)/2;
        System.out.println(round);

        for (int i = 0; i < round; i++){
            for (int q = i; q <= n - i - 2; q++){  //0-   [0,0][0,1]
                result.add(matrix[i][q]);
            }
            for (int j = i; j <= m - i - 2; j++){  //0-0   [0,2][1,2]
                result.add(matrix[j][n - i - 1]);
            }
            for (int k = n - i - 1; k > i; k--){  //2-1  [2,2][2,1]
                result.add(matrix[m - i - 1][k]);
            }
            for (int l = m - i - 1; l > i ; l-- ){                           
                result.add(matrix[l][i]);
            }
        }

        if (Math.min(m,n) % 2 == 1){
            int remain = Math.abs(n - m) + 1;
            int row = Math.min(m,n) / 2;
            int column = Math.min(m,n) / 2;
            if (m < n){
                for (int i = 0; i < remain; i++){
                    result.add(matrix[row][column]);
                    column++;
                }              
            }
            else if (m > n){
                for (int j = 0; j < remain; j++){
                    result.add(matrix[row][column]);
                    row++;
                }
            }
            else{
                result.add(matrix[row][column]);
            }
        }

        return result;
    }
}
```
