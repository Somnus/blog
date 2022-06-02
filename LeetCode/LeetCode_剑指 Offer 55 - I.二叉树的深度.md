<p>输入一棵二叉树的根节点，求该树的深度。从根节点到叶节点依次经过的节点（含根、叶节点）形成树的一条路径，最长路径的长度为树的深度。</p>

<p>例如：</p>

<p>给定二叉树 <code>[3,9,20,null,null,15,7]</code>，</p>

<pre>    3
   / \
  9  20
    /  \
   15   7</pre>

<p>返回它的最大深度&nbsp;3 。</p>

<p>&nbsp;</p>

<p><strong>提示：</strong></p>

<ol>
	<li><code>节点总数 &lt;= 10000</code></li>
</ol>

<p>注意：本题与主站 104&nbsp;题相同：<a href="https://leetcode-cn.com/problems/maximum-depth-of-binary-tree/">https://leetcode-cn.com/problems/maximum-depth-of-binary-tree/</a></p>

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
    public int MaxDepth(TreeNode root) {
        if(root == null)
            return 0;

        Queue<TreeNode> queue = new Queue<TreeNode>();
        queue.Enqueue(root);

        int height = 0;
        int count = 1;

        while(count > 0){
            TreeNode node = queue.Dequeue();
            
            if(node.left != null)
                queue.Enqueue(node.left);         
            if(node.right != null)
                queue.Enqueue(node.right);

           count -= 1;

           if(count == 0){
               count = queue.Count;
               height += 1;
           }
        }

        return height;
    }
}
```