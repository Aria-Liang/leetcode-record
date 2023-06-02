# Day23 BinaryTree 09

## Related Questions:
### [669. Trim a Binary Search Tree](https://leetcode.com/problems/trim-a-binary-search-tree/description/)
```
class Solution {
    public TreeNode trimBST(TreeNode root, int low, int high) {
        if (root == null){
            return root;
        }

        if (root.val > high){
            return trimBST(root.left, low, high);
        }

        else if(root.val < low){
            return trimBST(root.right, low, high);
        }

        else{
            root.left = trimBST(root.left, low, high);
            root.right = trimBST(root.right, low, high);
        }
        
        return root;
    }
}
```

### [108. Convert Sorted Array to Binary Search Tree](https://leetcode.com/problems/convert-sorted-array-to-binary-search-tree/description/)
```
class Solution {
    public TreeNode sortedArrayToBST(int[] nums) {
        if (nums.length == 0 || nums == null){
            return null;
        }

        return helper(nums, 0, nums.length);    
    }

    private TreeNode helper(int[] nums, int start, int end){
        System.out.println(start);
        System.out.println(end);
        if (start + 1 >= end - 1){                
            TreeNode temp = new TreeNode(nums[end - 1]);
            if (start != end - 1){
                temp.left = new TreeNode(nums[start]);
            }
            return temp;
        }
        
        int middle = start + (end - start - 1)/2;
        TreeNode root = new TreeNode(nums[middle]);
        root.left = helper(nums, start, middle);
        root.right = helper(nums, middle + 1, end);

        return root;
    }
}
```

### [538. Convert BST to Greater Tree](https://leetcode.com/problems/convert-bst-to-greater-tree/description/)
```
class Solution {
    int sum = 0;
    public TreeNode convertBST(TreeNode root) {
        if (root == null){
            return root;
        }

        convertBST(root.right);
        root.val = root.val + sum;
        sum = root.val;
        convertBST(root.left);

        return root;

    }
}
```
