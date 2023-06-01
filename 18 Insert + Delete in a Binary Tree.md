# Day22 BinaryTree 08

## Related Questions:
### [235. Lowest Common Ancestor of a Binary Search Tree](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/)
```
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if (root == null){
            return root;
        }

        if (root.val > p.val && root.val > q.val){
            return lowestCommonAncestor(root.left, p, q);
        }

        if (root.val <p.val && root.val < q.val){
            return lowestCommonAncestor(root.right, p, q);
        }

        return root;
    }
}
```

### [701. Insert into a Binary Search Tree](https://leetcode.com/problems/insert-into-a-binary-search-tree/)
```
class Solution {
    public TreeNode insertIntoBST(TreeNode root, int val) {
        if (root == null){
            return new TreeNode(val);
        }

        if (root.val > val){
            root.left = insertIntoBST(root.left, val);
        }
        if (root.val < val){
            root.right = insertIntoBST(root.right, val);
        }

        return root;

    }
}
```

### [450. Delete Node in a BST](https://leetcode.com/problems/delete-node-in-a-bst/description/)
```
class Solution {
    public TreeNode deleteNode(TreeNode root, int key) {
        if (root == null){
            return root;
        }

        if (root.val == key){
            return helper(root);
        }
        else if (root.val > key){
            root.left = deleteNode(root.left, key);
           
        }
        else{
            root.right = deleteNode(root.right, key);
        }

        return root;
    }

    private TreeNode helper(TreeNode root){
        if (root.left == null && root.right == null){return null;}
        else if (root.right == null){return root.left;}
        else if (root.left == null){return root.right;}   
        else{
            TreeNode parent = root.right;

            while (parent.left != null){
                parent = parent.left;
            }

            root.val = parent.val;
            root.right = deleteNode(root.right, parent.val);

            return root;
        }
    }
}
```
