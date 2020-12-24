<p>给定一棵二叉树，想象自己站在它的右侧，按照从顶部到底部的顺序，返回从右侧所能看到的节点值。</p>

<p><strong>示例:</strong></p>

<pre><strong>输入:</strong>&nbsp;[1,2,3,null,5,null,4]
<strong>输出:</strong>&nbsp;[1, 3, 4]
<strong>解释:
</strong>
   1            &lt;---
 /   \
2     3         &lt;---
 \     \
  5     4       &lt;---
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
    public IList<int> RightSideView(TreeNode root) {
        IList<int> list = new List<int>();
        if (root == null) return list;

        Queue<TreeNode> treeNodes = new Queue<TreeNode>();
        treeNodes.Enqueue(root);
        while (treeNodes.Count > 0)
        {
            int count = treeNodes.Count;
            while (count-- > 0)
            {
                TreeNode treeNode = treeNodes.Dequeue();
                if (treeNode.left != null) treeNodes.Enqueue(treeNode.left);
                if (treeNode.right != null) treeNodes.Enqueue(treeNode.right);
                if (count == 0) list.Add(treeNode.val);
            }
        }

        return list;
    }
}
```