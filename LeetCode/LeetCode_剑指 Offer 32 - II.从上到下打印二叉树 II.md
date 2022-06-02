<p>从上到下按层打印二叉树，同一层的节点按从左到右的顺序打印，每一层打印到一行。</p>

<p>&nbsp;</p>

<p>例如:<br>
给定二叉树:&nbsp;<code>[3,9,20,null,null,15,7]</code>,</p>

<pre>    3
   / \
  9  20
    /  \
   15   7
</pre>

<p>返回其层次遍历结果：</p>

<pre>[
  [3],
  [9,20],
  [15,7]
]
</pre>

<p>&nbsp;</p>

<p><strong>提示：</strong></p>

<ol>
	<li><code>节点总数 &lt;= 1000</code></li>
</ol>

<p>注意：本题与主站 102 题相同：<a href="https://leetcode-cn.com/problems/binary-tree-level-order-traversal/">https://leetcode-cn.com/problems/binary-tree-level-order-traversal/</a></p>

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
    public IList<IList<int>> LevelOrder(TreeNode root) {
                var list = new List<IList<int>>();      
        if(root == null) return list;

        Queue<TreeNode> queue = new Queue<TreeNode>();
        queue.Enqueue(root);

        var midList = new List<int>();  
        int count = 1;
        TreeNode midNode;
        while(count-- > 0){
            midNode = queue.Dequeue();

            if(midNode.left != null) queue.Enqueue(midNode.left);
            if(midNode.right != null) queue.Enqueue(midNode.right);

            midList.Add(midNode.val);

            if(count == 0){
                list.Add(midList);
                midList = new List<int>();
                count = queue.Count;
            }
        }
        return list;
    }
}
```