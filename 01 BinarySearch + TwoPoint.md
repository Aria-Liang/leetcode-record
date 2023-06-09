# Day1 Array01

##  Storage in Memory
Array elements are always stored in consecutive memory locations, but for 2-d array it is not consecutive

##  Basic Knowledge about array
- Instantiating an Array in java: 
`int[] intArray = new int[20];` (declaring array and allocate memory to array)
`int[][] arr = new int[3][3];`

- Array Literals:
`int[] intArray = new int[] {1,2,3,4,5};`
- For loop:
```
for (int i = 0; i < mylist.length; i++){
    total += mylist[i];
}

for (double element: mylist){
    System.out.println(element);
}
```
- Change Array to ArrayList:
```
List arrList = new ArrayList(Arrays.asList(intArray));
arrList.add(x);
```

## Related Questions:
### Binary Search
#### [704. Binary Search](https://leetcode.com/problems/binary-search/):  easy

#### [35. Search Insert Position](https://leetcode.com/problems/search-insert-position/description/): easy 

#### [34. Find First and Last Position of Element in Sorted Array](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/description/): first find the first position, and then find the last position


### Two Pointer
#### [27. Remove Element](https://leetcode.com/problems/remove-element/description/)

1.暴力解法： 每次发现val就把所有元素统一向前移动一格，同时size - 1
```
class Solution {
    public int removeElement(int[] nums, int val) {
        int size = nums.length;
        for (int i = 0; i < size; i++){
            if (nums[i] == val){
                for (int j = i + 1; j < nums.length; j++){
                    nums[j - 1] = nums[j]; 
                }
                i --;
                size --;
            }
        }
        return size;
    }
}
```
2. Two pointer - 同向指针: 
快指针遍历整个array, 慢指针指向non-val的下一个，快指针遍历一遍之后会把所有non-val值赋给慢指针
```
class Solution {
    public int removeElement(int[] nums, int val) {
        int slow = 0;

        for (int fast = 0; fast < nums.length; fast++){
            if (nums[fast] != val){
                nums[slow++] = nums[fast];
            }
        }

        return slow;
    }
}
```

3. Two pointer - 双向指针：
```
class Solution {
    public int removeElement(int[] nums, int val) {
        if (nums.length == 0 || nums == null){
            return 0;
        }

        int left = 0;
        int right = nums.length - 1;

        while (left < right){
            while (nums[left] != val && left < nums.length - 1){
                left ++;
            }
            while (nums[right] == val && right > 0){
                right --;
            }

            if (nums[left] == val && nums[right] != val && left < right){
                nums[left] = nums[right];
                nums[right] = val;
                left += 1;
                right -= 1;
            }
        }

        if (nums[left] != val){
            return left + 1;
        }
        return left;
    }
}
```
