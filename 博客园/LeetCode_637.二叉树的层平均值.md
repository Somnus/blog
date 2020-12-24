<p>给定一个非空二叉树, 返回一个由每层节点平均值组成的数组。</p>

<p>&nbsp;</p>

<p><strong>示例 1：</strong></p>

<pre><strong>输入：</strong>
    3
   / \
  9  20
    /  \
   15   7
<strong>输出：</strong>[3, 14.5, 11]
<strong>解释：</strong>
第 0 层的平均值是 3 ,  第1层是 14.5 , 第2层是 11 。因此返回 [3, 14.5, 11] 。
</pre>

<p>&nbsp;</p>

<p><strong>提示：</strong></p>

<ul>
	<li>节点值的范围在32位有符号整数范围内。</li>
</ul>

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
    public IList<double> AverageOfLevels(TreeNode root) {
        var list = new List<double>();
        if(root == null) return list;

        Queue<TreeNode> queue = new Queue<TreeNode>();
        queue.Enqueue(root);

        int height = 1;
        int count = 0;
        double sum = 0;
        TreeNode node;

        while(height-- > 0){
            node = queue.Dequeue();

            sum += node.val;
            count += 1;

            if(node.left != null) queue.Enqueue(node.left);
            if(node.right != null) queue.Enqueue(node.right);

            if(height == 0){
                list.Add(sum / count);
                count = 0;
                sum = 0;
                height = queue.Count;
            }
        }

        return list;
    }
}
```