# Day21 BinaryTree 07

## Related Questions:
### [530. Minimum Absolute Difference in BST](https://leetcode.com/problems/minimum-absolute-difference-in-bst/description/)
```
class Solution {
    int min = Integer.MAX_VALUE;
    public int getMinimumDifference(TreeNode root) {
        helper(root);
        return min;
        

    }

    private void helper(TreeNode root){
        if (root == null){
            return;
        }
        
        int leftabs = Integer.MAX_VALUE;
        int rightabs = Integer.MAX_VALUE;

        if (root.left != null){
            TreeNode leftMax = Max(root.left);
            leftabs = Math.abs(leftMax.val - root.val);
        }
        if (root.right != null){
            TreeNode rightMin = Min(root.right);
            rightabs = Math.abs(rightMin.val - root.val);
        }

        min = Math.min(Math.min(leftabs, rightabs), min);

        helper(root.left);
        helper(root.right);
        
    }

    private TreeNode Max(TreeNode root){

        while (root.right != null){
            root = root.right;
        }
        return root;  
    }

    private TreeNode Min(TreeNode root){

        while (root.left != null){
            root = root.left;
        }

        return root;
    }
}
```
这样写太复杂了，in order traversal本身就排好了序，我们只需要稍微记录一下prev点就可以了
```
class Solution {
    int min = Integer.MAX_VALUE;
    TreeNode pre;
    public int getMinimumDifference(TreeNode root) {
        helper(root);
        return min;   
    }

    private void helper(TreeNode root){
        if (root == null){
            return;
        }

        helper(root.left);

        if (pre != null){
            min = Math.min((root.val - pre.val),min);
        }
        pre = root;
        
        helper(root.right);
    }
}
```
也可以用统一的迭代法写：
```
class Solution {
    public int getMinimumDifference(TreeNode root) {
        Stack<TreeNode> stack = new Stack<>();
        TreeNode pre = null;
        int result = Integer.MAX_VALUE;

        if (root != null){
            stack.push(root);
        }

        while (!stack.isEmpty()){
            TreeNode curr = stack.pop();
            if (curr != null){
                if (curr.right != null) stack.push(curr.right);
                stack.push(curr);
                stack.push(null);
                if (curr.left != null) stack.push(curr.left);
            }
            else{
                TreeNode temp = stack.pop();
                if (pre != null){
                    result = Math.min(result, temp.val - pre.val);
                }
                pre = temp;
            }
        }
        return result;
    }
}
```

### [501. Find Mode in Binary Search Tree](https://leetcode.com/problems/find-mode-in-binary-search-tree/description/)
```
class Solution {
    int maxcount = 1;
    public int[] findMode(TreeNode root) {
        Map<Integer, Integer> map = new HashMap<>();
        ArrayList<Integer> result = new ArrayList<>();
        helper(root, map);
        for (Integer key: map.keySet()){
            if (map.get(key) == maxcount){
                result.add(key);
            }
        }
        int[] output = new int[result.size()];
        for(int i=0; i<result.size(); i++) {
            output[i] = result.get(i);
        }
        
        return output; 
    }

    private void helper(TreeNode root, Map<Integer, Integer> map){
        if (root == null){
            return;
        }

        if (map.containsKey(root.val)){
            int value = map.get(root.val) + 1;
            map.put(root.val, value);
            maxcount = Math.max(maxcount, value);
        }
        else{
            map.put(root.val, 1);
        }

        helper(root.left, map);
        helper(root.right, map);

    }
}
```
这是暴力解法，如果不想要额外占用空间，并且只想遍历一遍呢：
```
class Solution {
    ArrayList<Integer> resList;
    int maxcount;
    int count;
    TreeNode pre;

    public int[] findMode(TreeNode root) {
        resList = new ArrayList<>();
        maxcount = 0;
        count = 0;
        pre = null;
        helper(root);
        int[] res = new int[resList.size()];
        for (int i = 0; i < resList.size(); i++){
            res[i] = resList.get(i);
        }
        return res;
    }

    private void helper(TreeNode root){
        if (root == null){
            return;
        }

        helper(root.left);
        int rootvalue = root.val;

        if (pre == null || rootvalue != pre.val){
            count = 1;
        }
        else{
            count += 1;
        }

        if (count > maxcount){
            resList.clear();
            resList.add(rootvalue);
            maxcount = count;
        }
        else if (count == maxcount){
            resList.add(rootvalue);
        }

        pre = root;

        helper(root.right);
    }
}
```
通过找到之后clear list

### [236. Lowest Common Ancestor of a Binary Tree](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/description/)
```
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if (root == null){
            return root;
        }

        TreeNode left = lowestCommonAncestor(root.left, p, q);
        TreeNode right = lowestCommonAncestor(root.right, p, q);

        if (root.val == p.val || root.val == q.val){
            return root;
        }

        if (left != null && right != null){
            return root;
        }

        return (left != null? left: right);
    }

}
```
后续遍历就是天然的从下往上
