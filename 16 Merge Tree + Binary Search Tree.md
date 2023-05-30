# Day6 BinaryTree 06

## Related Questions:
### [654. Maximum Binary Tree](https://leetcode.com/problems/maximum-binary-tree/description/)
```
class Solution {
    public TreeNode constructMaximumBinaryTree(int[] nums) {
        if (nums.length == 0){
            return null;
        }
        return helper(nums, 0, nums.length);
    }

    private TreeNode helper(int[] nums, int start, int end){
        if (end == start){
            return null;
        }
        
        int middleIndex = findMax(nums, start, end);
        TreeNode root = new TreeNode(nums[middleIndex]);
        int leftstart = start;
        int leftend = middleIndex;
        int rightstart = middleIndex + 1;
        int rightend = end;

        root.left = helper(nums, leftstart, leftend);
        root.right = helper(nums, rightstart, rightend);

        return root;

    }

    private int findMax(int[] nums, int start, int end){
        int max_num = Integer.MIN_VALUE;
        int index = -1;

        for (int i = start; i < end; i++){
            if (nums[i] > max_num){
                max_num = nums[i];
                index = i;
            }
        }

        return index;
    }
}
```
和construct一个思路

### [617. Merge Two Binary Trees](https://leetcode.com/problems/merge-two-binary-trees/description/)
```
class Solution {
    public TreeNode mergeTrees(TreeNode root1, TreeNode root2) {
        if (root1 != null && root2 != null){
            TreeNode newRoot = new TreeNode(root1.val + root2.val);
            newRoot.left = mergeTrees(root1.left, root2.left);
            newRoot.right = mergeTrees(root1.right, root2.right);
            return newRoot;
        }

        else if (root1 != null && root2 == null){
            TreeNode newRoot = new TreeNode(root1.val);
            newRoot.left = mergeTrees(root1.left, null);
            newRoot.right = mergeTrees(root1.right, null);
            return newRoot;
        }

        else if (root2 != null && root1 == null){
            TreeNode newRoot = new TreeNode(root2.val);
            newRoot.left = mergeTrees(root2.left, null);
            newRoot.right = mergeTrees(root2.right, null);
            return newRoot;
        }

        else{
            return null;
        }
    }
}
```
写的过于复杂
```
class Solution {
    // 递归
    public TreeNode mergeTrees(TreeNode root1, TreeNode root2) {
        if (root1 == null) return root2;
        if (root2 == null) return root1;

        root1.val += root2.val;
        root1.left = mergeTrees(root1.left,root2.left);
        root1.right = mergeTrees(root1.right,root2.right);
        return root1;
    }
}
```

```
class Solution {
    // 使用栈迭代
    public TreeNode mergeTrees(TreeNode root1, TreeNode root2) {
        if (root1 == null) {
            return root2;
        }
        if (root2 == null) {
            return root1;
        }
        Stack<TreeNode> stack = new Stack<>();
        stack.push(root2);
        stack.push(root1);
        while (!stack.isEmpty()) {
            TreeNode node1 = stack.pop();
            TreeNode node2 = stack.pop();
            node1.val += node2.val;
            if (node2.right != null && node1.right != null) {
                stack.push(node2.right);
                stack.push(node1.right);
            } else {
                if (node1.right == null) {
                    node1.right = node2.right;
                }
            }
            if (node2.left != null && node1.left != null) {
                stack.push(node2.left);
                stack.push(node1.left);
            } else {
                if (node1.left == null) {
                    node1.left = node2.left;
                }
            }
        }
        return root1;
    }
}
```
也可以用stack, 因为如果走到某一个tree的left node或者right node为null, 就没什么必要走了， 所以后续也不用加入stack

### [700. Search in a Binary Search Tree](https://leetcode.com/problems/search-in-a-binary-search-tree/description/)
利用binary search的递归方法：
```
class Solution {
    public TreeNode searchBST(TreeNode root, int val) {
        if (root == null){
            return null;
        }

        if (root.val > val){
            return searchBST(root.left, val);
        }
        else if(root.val == val){
            return root;
        }
        else{
            return searchBST(root.right, val);
        }
    }
}
```
不利用binary search的递归方法：
```
class Solution {
    // 递归，普通二叉树
    public TreeNode searchBST(TreeNode root, int val) {
        if (root == null || root.val == val) {
            return root;
        }

        TreeNode left = searchBST(root.left, val);
        if (left != null) return left;    //如果左边找不到，有可能没有，有可能在右边

        TreeNode right = searchBST(root.right, val);
        
        return right;
    }
}

```
```
class Solution {
    // 迭代，普通二叉树
    public TreeNode searchBST(TreeNode root, int val) {
        if (root == null || root.val == val) {
            return root;
        }
        Stack<TreeNode> stack = new Stack<>();
        stack.push(root);
        while (!stack.isEmpty()) {
            TreeNode pop = stack.pop();
            if (pop.val == val) {
                return pop;
            }
            if (pop.right != null) {
                stack.push(pop.right);
            }
            if (pop.left != null) {
                stack.push(pop.left);
            }
        }
        return null;
    }
}

class Solution {
    // 迭代，利用二叉搜索树特点，优化，可以不需要栈
    public TreeNode searchBST(TreeNode root, int val) {
        while (root != null)
            if (val < root.val) root = root.left;
            else if (val > root.val) root = root.right;
            else return root;
        return null;
    }
}
```

### [98. Validate Binary Search Tree](https://leetcode.com/problems/validate-binary-search-tree/description/)
```
class ReturnType{
    boolean balanced;
    int minnum;
    int maxnum;
    public ReturnType(boolean balanced, int minnum, int maxnum){
        this.balanced = balanced;
        this.minnum = minnum;
        this.maxnum = maxnum;
    }
}
class Solution {
    public boolean isValidBST(TreeNode root) {
        ReturnType result = helper(root);
        return result.balanced;
    }

    private ReturnType helper(TreeNode root){
        if (root == null){
            return new ReturnType(true, Integer.MIN_VALUE, Integer.MAX_VALUE);
        }

        if (root.left == null && root.right == null){
            return new ReturnType(true, root.val, root.val);
        }

        ReturnType left = helper(root.left);
        ReturnType right = helper(root.right);

        if (root.left == null){
            if (right.minnum > root.val && right.balanced == true){
                return new ReturnType(true, root.val, right.maxnum);
            }
        }

        if (root.right == null){
            if (left.maxnum < root.val && left.balanced == true){
                return new ReturnType(true, left.minnum, root.val);
            }
        }
        
        if (left.balanced == true && right.balanced == true && left.maxnum < root.val && right.minnum > root.val){
            return new ReturnType(true, left.minnum, right.maxnum);
        }
            
        return new ReturnType(false, -1, -1);
        

    }
}
```
写的略微复杂，也可以直接整体法递归，因为BFS有个重要特性，如果in order  traversal会是从小到大的。
```
//方法1：
class Solution {
    public boolean isValidBST(TreeNode root) {
        Stack<TreeNode> stack = new Stack<>();
        TreeNode pre = null;
        if(root != null)
            stack.add(root);        
        while(!stack.isEmpty()){
            TreeNode curr = stack.peek();
            if(curr != null){
                stack.pop();
                if(curr.right != null)
                    stack.add(curr.right);
                stack.add(curr);
                stack.add(null);
                if(curr.left != null)
                    stack.add(curr.left);     //第一步相当于是把数字都展开，以in order的顺序展开
            }else{
                stack.pop();
                TreeNode temp = stack.pop();
                if(pre != null && pre.val >= temp.val)     //处理展开之后的以in order顺序排列的数字，prev应该小于curr
                    return false;
                pre = temp;
            }
        }
        return true;
    }
}

// 递归
class Solution {
    TreeNode max;
    public boolean isValidBST(TreeNode root) {
        if (root == null) {
            return true;
        }
        // 左
        boolean left = isValidBST(root.left);
        if (!left) {
            return false;
        }
        // 中
        if (max != null && root.val <= max.val) {                 //这里不需要比较root.val小于right tree最小值的原因是，in order traversal会自动像爬坡一样从最小值爬到最大值
            return false;                                         //然后我们只用更新每次的max value就可以
        }
        max = root;
        // 右
        boolean right = isValidBST(root.right);
        return right;
    }
}
```
