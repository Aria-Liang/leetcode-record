# Day15 BinaryTree 02

## Related Questions:
### [102. Binary Tree Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal/description/)
```
class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> result = new ArrayList<>();
        Queue<TreeNode> q1 = new LinkedList<>();
       
        if (root == null){
            return result;
        }

        q1.offer(root);
        
        while (!q1.isEmpty()){
            List<Integer> list = new ArrayList<>();
            int len = q1.size();
            for (int i = 0; i < len; i++){
                TreeNode node = q1.poll();
                list.add(node.val);
                if (node.left != null) q1.offer(node.left);    //注意要判断是否是null, 不然可能会把null放进去，导致后面null.val出错
                if (node.right != null) q1.offer(node.right);
            }
            result.add(list);
        }

        return result;
    }
}
```

### [107. Binary Tree Level Order Traversal II](https://leetcode.com/problems/binary-tree-level-order-traversal-ii/description/)
```
class Solution {
    public List<List<Integer>> levelOrderBottom(TreeNode root) {
        List<List<Integer>> unreverse = new ArrayList<>();
        Queue<TreeNode> q1 = new LinkedList<>();

        if (root == null){
            return unreverse;
        }

        q1.offer(root);

        while(!q1.isEmpty()){
            List<Integer> temp = new ArrayList<>();
            int len = q1.size();

            for (int i = 0; i < len; i++){
                TreeNode node = q1.poll();
                temp.add(node.val);
                if (node.left != null) q1.add(node.left);
                if (node.right != null) q1.add(node.right);
            }

            unreverse.add(temp);
        }

        Collections.reverse(unreverse);
        return unreverse;
    }
}
```
##### 可以进行优化：
##### 利用链表可以进行 O(1) 头部插入, 这样最后答案不需要再反转
```
LinkedList<List<Integer>> ans = new LinkedList<>();
ans.addFirst(temp);
return ans; //这样直接返回也可以
```
#### [199. Binary Tree Right Side View](https://leetcode.com/problems/binary-tree-right-side-view/description/)
```
class Solution {
    public List<Integer> rightSideView(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        Queue<TreeNode> q1 = new LinkedList<>();

        if (root == null){
            return result;
        }

        q1.offer(root);

        while (!q1.isEmpty()){
            int len = q1.size();
            
            for (int i = 0; i < len - 1; i++){
                TreeNode node = q1.poll();
                if (node.left != null) q1.offer(node.left);
                if (node.right != null) q1.offer(node.right);
            }

            TreeNode last = q1.poll();
            result.add(last.val);
            if (last.left != null) q1.offer(last.left);
            if (last.right != null) q1.offer(last.right);
        }
        
        return result;
    }
}
```
代码有些许重复，可以优化
```
while (!que.isEmpty()) {
            int levelSize = que.size();

            for (int i = 0; i < levelSize; i++) {
                TreeNode poll = que.pollFirst();

                if (poll.left != null) {
                    que.addLast(poll.left);
                }
                if (poll.right != null) {
                    que.addLast(poll.right);
                }

                if (i == levelSize - 1) {
                    list.add(poll.val);
                }
            }
        }

        return list;
```
#### [637. Average of Levels in Binary Tree](https://leetcode.com/problems/average-of-levels-in-binary-tree/description/)
```
class Solution {
    public List<Double> averageOfLevels(TreeNode root) {
        List<Double> result = new ArrayList<>();
        Queue<TreeNode> q1 = new LinkedList<>();

        if (root == null){
            return result;
        }

        q1.offer(root);

        while (!q1.isEmpty()){
            int len = q1.size();
            long sum = 0;
            for (int i = 0; i < len; i++){
                TreeNode node = q1.poll();
                sum += node.val;
                if (node.left != null) q1.offer(node.left);
                if (node.right != null) q1.offer(node.right);
            }
            
            double avg = (double)sum/len;
            result.add(avg);
        }

        return result;
    }
}
```

#### [429. N-ary Tree Level Order Traversal](https://leetcode.com/problems/n-ary-tree-level-order-traversal/description/)
```
class Solution {
    public List<List<Integer>> levelOrder(Node root) {
        List<List<Integer>> result = new ArrayList<>();
        Queue<Node> q1 = new LinkedList<>();

        if (root == null){
            return result;
        }

        q1.offer(root);

        while (!q1.isEmpty()){
            int len = q1.size();
            List<Integer> list = new ArrayList<>();

            for (int i = 0; i < len; i++){
                Node node = q1.poll();
                list.add(node.val);

                for (Node child: node.children){
                    if (child != null){
                        q1.offer(child);
                    }
                }
            }
            result.add(list);
        } 

        return result;  
    }
}
```

#### [515. Find Largest Value in Each Tree Row](https://leetcode.com/problems/find-largest-value-in-each-tree-row/description/)
```
class Solution {
    public List<Integer> largestValues(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        Queue<TreeNode> q1 = new LinkedList<>();

        if (root == null){
            return result;
        }

        q1.offer(root);

        while (!q1.isEmpty()){
            int len = q1.size();
            int max = Integer.MIN_VALUE;

            for (int i = 0; i < len; i++){
                TreeNode node = q1.poll();
                max = Math.max(max, node.val);
                if (node.left != null) q1.offer(node.left);
                if (node.right != null) q1.offer(node.right);
            }
            result.add(max);
        }

        return result;
    }
}
```

#### [104. Maximum Depth of Binary Tree](https://leetcode.com/problems/maximum-depth-of-binary-tree/description/)
```
class Solution {
    public int maxDepth(TreeNode root) {
        int depth = 0;
        Queue<TreeNode> q1 = new LinkedList<>();

        if (root == null){
            return depth;
        }

        q1.offer(root);

        while (!q1.isEmpty()){
            int len = q1.size();
            for (int i = 0; i < len; i++){
                TreeNode node = q1.poll();
                if (node.left != null) q1.offer(node.left);
                if (node.right != null) q1.offer(node.right);
            }
            depth += 1;
        }

        return depth;
    }
}
```
#### [111. Minimum Depth of Binary Tree](https://leetcode.com/problems/minimum-depth-of-binary-tree/description/)
```
class Solution {
    public int minDepth(TreeNode root) {
        int min_depth = 0;
        Queue<TreeNode> q1 = new LinkedList<>();

        if (root == null){
            return min_depth;
        }
        q1.offer(root);

        while(!q1.isEmpty()){
            int len = q1.size();
            for (int i = 0; i < len; i++){
                TreeNode node = q1.poll();
                if (node.left == null && node.right == null){
                    return min_depth + 1;
                }
                if (node.left != null) q1.offer(node.left);
                if (node.right != null) q1.offer(node.right);
            }
            min_depth += 1;
        }

        return min_depth;
    }
}
```
_________________________________________________________________________________________________________________________________________________________________________

#### [226. Invert Binary Tree](https://leetcode.com/problems/invert-binary-tree/description/)
```
class Solution {
    public TreeNode invertTree(TreeNode root) {
        if (root == null){
            return root;
        }

        TreeNode left = invertTree(root.left);
        TreeNode right = invertTree(root.right);

        root.left = right;
        root.right = left;

        return root;

    }
}
```
##### 其实是一种后续遍历, DFS
##### 也可以用BFS做

```
class Solution {
    public TreeNode invertTree(TreeNode root) {
        Queue<TreeNode> q1 = new LinkedList<>();

        if (root == null){
            return root;
        }

        q1.offer(root);     //别忘了加进q1里
        while (!q1.isEmpty()){
            int size = q1.size();
            for (int i = 0; i < size; i++){
                TreeNode node = q1.poll();
                TreeNode left = node.left;
                TreeNode right = node.right;
                node.left = right;
                node.right = left;
                if (left != null) q1.offer(left);
                if (right != null) q1.offer(right);
            }
        }

        return root;
    }
}
```
_______________________________________________________________________________________________________________________
#### [101. Symmetric Tree](https://leetcode.com/problems/symmetric-tree/)
###### 这题一开始尝试了两种思路，一种是分制法一种是层级遍历都出了一些问题，总结如下：
###### 分治法：比较了isSymmetric(root.left) 和 isSymmetrc(root.right)， 然后left.val == right.val，但是这个比较的其实是两边子树是不是完全相等，这里对称其实是翻转，所以需要对比外侧和内侧
###### 层级遍历：这题不可以用层级遍历，因为[1,1,null,null]这种也会被认为是符合要求的，层级遍历无法确定具体是哪个位置
```
class Solution {
    public boolean isSymmetric(TreeNode root) {
        if (root == null){
            return true;
        }
        return compare(root.left, root.right); 
    }

    private boolean compare(TreeNode left, TreeNode right){
        if (left == null && right == null){
            return true;
        }

        if (left == null || right == null){
            return false;
        }

        if (left.val != right.val){
            return false;
        }

        boolean outside = compare(left.left, right.right);
        boolean inside = compare(left.right, right.left);
        return outside && inside;
    }
}
```
仍然可以用迭代法： 即queue
如果用deque的话是使用双端队列，相当于两个栈
```
public boolean isSymmetric2(TreeNode root) {
        Deque<TreeNode> deque = new LinkedList<>();
        deque.offerFirst(root.left);
        deque.offerLast(root.right);
        while (!deque.isEmpty()) {
            TreeNode leftNode = deque.pollFirst();
            TreeNode rightNode = deque.pollLast();
            if (leftNode == null && rightNode == null) {
                continue;
            }
        if (leftNode == null || rightNode == null || leftNode.val != rightNode.val) {
                return false;
            }
            deque.offerFirst(leftNode.left);
            deque.offerFirst(leftNode.right);
            deque.offerLast(rightNode.right);
            deque.offerLast(rightNode.left);
        }
        return true;
    }
```
也可以用单个迭代法，只是加入进去的顺序会变
```
 public boolean isSymmetric3(TreeNode root) {
        Queue<TreeNode> deque = new LinkedList<>();
        deque.offer(root.left);
        deque.offer(root.right);
        while (!deque.isEmpty()) {
            TreeNode leftNode = deque.poll();
            TreeNode rightNode = deque.poll();

            if (leftNode == null || rightNode == null || leftNode.val != rightNode.val) {
                return false;
            }
            // 这里顺序与使用Deque不同
            deque.offer(leftNode.left);
            deque.offer(rightNode.right);
            deque.offer(leftNode.right);
            deque.offer(rightNode.left);
        }
        return true;
    }
```

还有两题也属于层级遍历的题目还没做：116,117
