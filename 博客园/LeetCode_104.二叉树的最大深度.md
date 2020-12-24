<p>给定一个二叉树，找出其最大深度。</p>

<p>二叉树的深度为根节点到最远叶子节点的最长路径上的节点数。</p>

<p><strong>说明:</strong>&nbsp;叶子节点是指没有子节点的节点。</p>

<p><strong>示例：</strong><br>
给定二叉树 <code>[3,9,20,null,null,15,7]</code>，</p>

<pre>    3
   / \
  9  20
    /  \
   15   7</pre>

<p>返回它的最大深度&nbsp;3 。</p>

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
            
            if(node.left != null)queue.Enqueue(node.left);         
            if(node.right != null)queue.Enqueue(node.right);

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