# Day18 BinaryTree 05

## Related Questions:
### [513. Find Bottom Left Tree Value](https://leetcode.com/problems/find-bottom-left-tree-value/description/)
```
class Solution {
    public int findBottomLeftValue(TreeNode root) {
        List<Integer> level = new ArrayList<>();
        Queue<TreeNode> q1 = new LinkedList<>();

        if (root == null){
            return -1;
        }

        q1.offer(root);

        while (!q1.isEmpty()){
            int len = q1.size();
            level = new ArrayList<>();
            for (int i = 0; i < len; i++){
                TreeNode node = q1.poll();
                level.add(node.val);
                if (node.left != null) q1.offer(node.left);
                if (node.right != null) q1.offer(node.right);
            }
        }

        return level.get(0);
    }
}
```
这题毫无疑问层序遍历很简单，但是可以优化的地方是：不用记录level这个list of integer, 可以直接记录每一个level第一个元素就可以
```
class Solution {

    public int findBottomLeftValue(TreeNode root) {
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        int res = 0;
        while (!queue.isEmpty()) {
            int size = queue.size();
            for (int i = 0; i < size; i++) {
                TreeNode poll = queue.poll();
                if (i == 0) {
                    res = poll.val;
                }
                if (poll.left != null) {
                    queue.offer(poll.left);
                }
                if (poll.right != null) {
                    queue.offer(poll.right);
                }
            }
        }
        return res;
    }
}
```

第二种方法是递归的做法：思路是找到deep最深的第一个元素
```
class Solution {
    int dept = 0;
    int num = -1;
    public int findBottomLeftValue(TreeNode root) {
        helper(root, 0);
        return num;
    }

    private void helper(TreeNode root, int dep){
        if (root == null){
            return;
        }

        dep+= 1;
        if (dep > dept){
            dept = dep;
            num = root.val;
        }

        helper(root.left, dep);
        helper(root.right, dep);
    }
}
```
  
### [112. Path Sum](https://leetcode.com/problems/path-sum/description/)
```
class Solution {
    public boolean hasPathSum(TreeNode root, int targetSum) {
        if (root == null){
            return false;
        }

        if (root.left == null && root.right == null){
            return root.val == targetSum;
        }

        boolean left = hasPathSum(root.left, targetSum - root.val);
        boolean right = hasPathSum(root.right, targetSum - root.val);

        return left || right;
    }
}
```
对于int而言，是pass by value, any change made to the value within a method will not affect the original outside of the method. 但是对于string, list 这种而言，相当于是pass by reference 的， change will be visible outside the method. 

### [113. Path Sum II](https://leetcode.com/problems/path-sum-ii/description/)
```
class Solution {
    public List<List<Integer>> pathSum(TreeNode root, int targetSum) {
        List<List<Integer>> result = new ArrayList<>();
        ArrayList<Integer> list = new ArrayList<>();
        helper(result, root, targetSum, list);
        return result;

    }

    private void helper(List<List<Integer>> result, TreeNode root, int targetSum, ArrayList<Integer> list){
        if (root == null){
            return;
        }

        list.add(root.val);

        if (root.left == null && root.right == null){
            if (root.val == targetSum){
               
                result.add(new ArrayList<>(list));
            }
            //return;
        }

        //list.add(root.val);

        helper(result, root.left, targetSum - root.val, list);
        helper(result, root.right, targetSum - root.val, list);
        list.remove(list.size() - 1);
    }
}
```
list 需要回溯

### [106. Construct Binary Tree from Inorder and Postorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/description/)
错误解法： 
```
class Solution {
    public TreeNode buildTree(int[] inorder, int[] postorder) {
        // in order: left middle right 9 3 15 20 7
        // post order: left right middle 9 15 7 20 3
        if (postorder.length == 0){
            return null;
        }

        TreeNode root = new TreeNode(postorder[postorder.length - 1]);
        if (postorder.length == 1){
            return root;
        }

        int[] inOrderLeft = new ArrayList<>();           // compilation error because you are trying to assign an ArrayList object to an int[] array variable, which are incompatible types.
        int[] inOrderRight = new ArrayList<>();
        int[] postOrderLeft = new ArrayList<>();
        int[] postOrderRight = new ArrayList<>();

        int sliceIndex = 0;
        for (int i = 0; i < inorder.length; i++){
            if (inorder[i] == root.val) {
                sliceIndex = i;
                break;
            }
        }

        inOrderLeft = inorder.subList(0, sliceIndex);
        inOrderRight = inorder.subList(sliceIndex + 1, inorder.length);

        postOrderLeft = postorder.subList(0, sliceIndex);
        postOrderRight = postorder.subList(sliceIndex, postorder.length - 1);

        root.left = buildTree(inOrderLeft, postOrderLeft);
        root.right = buildTree(inOrderRight, postOrderRight);

        return root;
    }
}
```
如果list无法slice, 可以考虑记录下来想要切割的点
```
class Solution {
    public TreeNode buildTree(int[] inorder, int[] postorder) {
        return helper(inorder, postorder, 0, inorder.length, 0, postorder.length);
    }

    private TreeNode helper(int[] inorder, int[] postorder, int inorderstart, int inorderend, int postorderstart, int postorderend){
        // in order: left middle right 9 3 15 20 7
        // post order: left right middle 9 15 7 20 3
        if (postorderend == postorderstart){
            return null;
        }

        TreeNode root = new TreeNode(postorder[postorderend - 1]);
        if (postorderend - postorderstart == 0){
            return root;
        }

        int sliceIndex = 0;
        for (int i = inorderstart; i < inorderend; i++){
            if (inorder[i] == root.val) {
                sliceIndex = i;
                break;
            }
        }

        int inOrderLeftStart = inorderstart;
        int inOrderLeftEnd = sliceIndex;
        int inOrderRightStart = sliceIndex + 1;
        int inOrderRightEnd = inorderend;

        int postOrderLeftStart = postorderstart;
        int postOrderLeftEnd = postorderstart + sliceIndex - inorderstart;
        int postOrderRightStart = postorderstart + sliceIndex - inorderstart;
        int postOrderRightEnd = postorderend - 1;

        root.left = helper(inorder, postorder, inOrderLeftStart, inOrderLeftEnd, postOrderLeftStart, postOrderLeftEnd);
        root.right = helper(inorder,postorder, inOrderRightStart, inOrderRightEnd, postOrderRightStart, postOrderRightEnd);

        return root;
    }
}
```
注意这里切割的时候要统一好是左闭右开，如果左闭右闭的话，无法讨论区间为空的情况

### [105. Construct Binary Tree from Preorder and Inorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)
```
class Solution {
    public TreeNode buildTree(int[] preorder, int[] inorder) {
        //preorder: 3, 9, 20, 15, 17
        //inorder: 9, 3, 15, 20, 7
        return helper(preorder, inorder, 0, preorder.length, 0, inorder.length);
    }

    private TreeNode helper(int[] preorder, int[] inorder, int prestart, int preend, int instart, int inend){
        if (preend == prestart){
            return null;
        }

        TreeNode root = new TreeNode(preorder[prestart]);

        int middleIndex = 0;
        for (int i = instart; i < inend; i++){
            if (inorder[i] == root.val){
                middleIndex = i;
                break;
            }
        }

        int preleftstart = prestart + 1;
        int preleftend = preleftstart + (middleIndex - instart);
        int prerightstart = preleftend;
        int prerightend = preend;
        
        int inleftstart = instart;
        int inleftend = middleIndex;
        int inrightstart = inleftend + 1;
        int inrightend = inend;

        root.left = helper(preorder, inorder, preleftstart, preleftend, inleftstart, inleftend);
        root.right = helper(preorder, inorder, prerightstart, prerightend, inrightstart, inrightend);

        return root;

    }
}
```
