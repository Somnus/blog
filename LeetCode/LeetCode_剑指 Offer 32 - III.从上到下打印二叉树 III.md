<p>请实现一个函数按照之字形顺序打印二叉树，即第一行按照从左到右的顺序打印，第二层按照从右到左的顺序打印，第三行再按照从左到右的顺序打印，其他行以此类推。</p>

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
  [20,9],
  [15,7]
]
</pre>

<p>&nbsp;</p>

<p><strong>提示：</strong></p>

<ol>
	<li><code>节点总数 &lt;= 1000</code></li>
</ol>

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
        while(count > 0){
            midNode = queue.Dequeue();

            if(midNode.left != null) queue.Enqueue(midNode.left);
            if(midNode.right != null) queue.Enqueue(midNode.right);

            midList.Add(midNode.val);
            
            count -= 1;
            if(count == 0){
                list.Add(midList);
                midList = new List<int>();
                count = queue.Count;
            }
        }

        for(int i = 0; i < list.Count; i++){
            if(i % 2 == 1) list[i] = list[i].Reverse().ToArray();
        }
        return list;
    }
}
```