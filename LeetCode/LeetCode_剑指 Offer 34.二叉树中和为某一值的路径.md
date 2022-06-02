<p>输入一棵二叉树和一个整数，打印出二叉树中节点值的和为输入整数的所有路径。从树的根节点开始往下一直到叶节点所经过的节点形成一条路径。</p>

<p>&nbsp;</p>

<p><strong>示例:</strong><br>
给定如下二叉树，以及目标和&nbsp;<code>sum = 22</code>，</p>

<pre>              <strong>5</strong>
             / \
            <strong>4</strong>   <strong>8</strong>
           /   / \
          <strong>11</strong>  13  <strong>4</strong>
         /  \    / \
        7    <strong>2</strong>  <strong>5</strong>   1
</pre>

<p>返回:</p>

<pre>[
   [5,4,11,2],
   [5,8,4,5]
]
</pre>

<p>&nbsp;</p>

<p><strong>提示：</strong></p>

<ol>
	<li><code>节点总数 &lt;= 10000</code></li>
</ol>

<p>注意：本题与主站 113&nbsp;题相同：<a href="https://leetcode-cn.com/problems/path-sum-ii/">https://leetcode-cn.com/problems/path-sum-ii/</a></p>

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
    public IList<IList<int>> PathSum(TreeNode root, int sum) {
        var list = new List<IList<int>>();
        if(root == null || root.left == null && root.right == null && root.val != sum) return list;
        if(root.left == null && root.right == null && root.val == sum) {
            list.Add(new List<int>(){ root.val });
            return list;
        }

        foreach(var item in PathSum(root.left, sum - root.val)){
            var midList = new List<int>() { root.val };
            midList.AddRange(item);
            list.Add(midList);
        }  

        foreach(var item in PathSum(root.right, sum - root.val)){
            var midList = new List<int>() { root.val };
            midList.AddRange(item);
            list.Add(midList);
        }     
        
        return list;
    }
}
```