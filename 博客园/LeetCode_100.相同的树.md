<p>给定两个二叉树，编写一个函数来检验它们是否相同。</p>

<p>如果两个树在结构上相同，并且节点具有相同的值，则认为它们是相同的。</p>

<p><strong>示例&nbsp;1:</strong></p>

<pre><strong>输入: </strong>      1         1
          / \       / \
         2   3     2   3

        [1,2,3],   [1,2,3]

<strong>输出:</strong> true</pre>

<p><strong>示例 2:</strong></p>

<pre><strong>输入:  </strong>    1          1
          /           \
         2             2

        [1,2],     [1,null,2]

<strong>输出:</strong> false
</pre>

<p><strong>示例&nbsp;3:</strong></p>

<pre><strong>输入:</strong>       1         1
          / \       / \
         2   1     1   2

        [1,2,1],   [1,1,2]

<strong>输出:</strong> false
</pre>

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
    public bool IsSameTree(TreeNode p, TreeNode q) {
        if (p == null && q == null) return true;
        if (p == null && q != null || p != null && q == null) return false;
        return p.val == q.val && IsSameTree(p.left, q.left) && IsSameTree(p.right, q.right);
    }
}
```