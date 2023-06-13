# Day35 Greedy Algorithm 04

## Related Questions:
### [860. Lemonade Change](https://leetcode.com/problems/lemonade-change/description/)
```
class Solution {
    public boolean lemonadeChange(int[] bills) {
        int five = 0;
        int ten = 0;

        for (int i = 0; i < bills.length; i++){
            if (bills[i] == 5){
                five += 1;
            }
            if (bills[i] == 10){
                ten += 1;
                five -= 1;
                if (five < 0){return false;}
            }
            if (bills[i] == 20){
                if (ten > 0 && five > 0){
                    ten -= 1;
                    five -= 1;
                }
                else if (ten == 0 && five >= 3){
                    five -= 3;
                }
                else{
                    return false;
                }
            }
        }
        return true;
    }
}
```

### [406. Queue Reconstruction by Height](https://leetcode.com/problems/queue-reconstruction-by-height/description/)
```
class Solution {
    public int[][] reconstructQueue(int[][] people) {
        Arrays.sort(people, (a, b) -> {
            if (a[0] == b[0]) return a[1] - b[1];
            return b[0] - a[0];
        });

        LinkedList<int[]> que = new LinkedList<>();

        for(int i = 0; i < people.length; i++){
            que.add(people[i][1], people[i]);
        }

        return que.toArray(new int[people.length][2]);
    }
}
```
- 这题也很难想，对于[hi, ki]两个变量，相当于是既要根据hi排序，又要根据ki排序。但是这题很鸡贼的是只有比hi大的值会影响ki的排序，所以我们如果先根据hi排序，就可以下一步ki为几就插入index为几，同时又不会影响前面的排序。
-------------------------------------------------------------------------------
- 技术难点：
- 1. int[][]根据第一个排序：
```
Arrays.sort(people, (a, b) -> {
            if (a[0] == b[0]) return a[1] - b[1];
            return b[0] - a[0];    // b - a 是降序排列， a - b 是升序排列
        });
```
- 这里（a，b）-> {}是lambda function
- Arrays.sort(nums, (a, b) -> b - a} 是降序排序
--------------------------------------------------
- 2. 用linkedlist进行insert时间复杂度为O(1)的操作
- 正常的linkedlist是add()自动到最后一个，如果插入specific index, the other part will be remain null, and if we convert into array, the null will still be null
- 但是这里不用担心，因为第一个元素按照排序是最大的元素，他的index肯定为0， 所以一开始就会被放在0

### [452. Minimum Number of Arrows to Burst Balloons](https://leetcode.com/problems/minimum-number-of-arrows-to-burst-balloons/description/)
```
class Solution {
    public int findMinArrowShots(int[][] points) {
        if (points.length <= 1){
            return points.length;
        }

        Arrays.sort(points, (a, b) -> { return a[0] - b[0]; });    //有溢出问题，改成Arrays.sort(points, (a, b) -> Integer.compare(a[0],b[0]));
        int left = points[0][0];
        int right = points[0][1];
        int num = 1;

        for (int i = 1; i < points.length; i++){
            int temp_left = points[i][0];
            int temp_right = points[i][1];
            System.out.println(temp_left <= right);
            if (temp_left <= right){
                right = Math.min(right, temp_right);
                left = Math.max(left, temp_left);
            }
            else{
                num += 1;
                left = temp_left;
                right = temp_right;
            }
        }
        return num;
    }
}
```
- 总有个测试点过不了[[-2147483646,-2147483645],[2147483646,2147483647]]，因为有溢出问题，原因出在 Arrays.sort(points, (a, b) -> { return a[0] - b[0]; });  
- 有个可以优化的地方，在排序之后其实左边界就不重要了，重要的是右边界
```
class Solution {
    public int findMinArrowShots(int[][] points) {
        // 根据气球直径的开始坐标从小到大排序
        // 使用Integer内置比较方法，不会溢出
        Arrays.sort(points, (a, b) -> Integer.compare(a[0], b[0]));

        int count = 1;  // points 不为空至少需要一支箭
        for (int i = 1; i < points.length; i++) {
            if (points[i][0] > points[i - 1][1]) {  // 气球i和气球i-1不挨着，注意这里不是>=
                count++; // 需要一支箭
            } else {  // 气球i和气球i-1挨着
                points[i][1] = Math.min(points[i][1], points[i - 1][1]); // 更新重叠气球最小右边界
            }
        }
        return count;
    }
}
```
