<p>给定一个二叉树，检查它是否是镜像对称的。</p>

<p>&nbsp;</p>

<p>例如，二叉树&nbsp;<code>[1,2,2,3,4,4,3]</code> 是对称的。</p>

<pre>    1
   / \
  2   2
 / \ / \
3  4 4  3
</pre>

<p>&nbsp;</p>

<p>但是下面这个&nbsp;<code>[1,2,2,null,3,null,3]</code> 则不是镜像对称的:</p>

<pre>    1
   / \
  2   2
   \   \
   3    3
</pre>

<p>&nbsp;</p>

<p><strong>进阶：</strong></p>

<p>你可以运用递归和迭代两种方法解决这个问题吗？</p>

### C#代码

```
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     public int val;
 *     public TreeNode left;
 *     public TreeNode right;
 *     public TreeNode(int x) { val = x; }
 * }
 */
public class Solution {
    public bool IsSymmetric(TreeNode root) {
        return IsSymmetric(root,root); 
    }
    public bool IsSymmetric(TreeNode p,TreeNode q) {
        if(p == null && q == null) return true;
        if(p == null || q == null) return false;
        if(p.val != q.val) return false;
        return IsSymmetric(p.right, q.left) && IsSymmetric(p.left, q.right);
    }
}
```