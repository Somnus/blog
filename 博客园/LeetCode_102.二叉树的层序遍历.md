<p>给你一个二叉树，请你返回其按 <strong>层序遍历</strong> 得到的节点值。 （即逐层地，从左到右访问所有节点）。</p>

<p>&nbsp;</p>

<p><strong>示例：</strong><br>
二叉树：<code>[3,9,20,null,null,15,7]</code>,</p>

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