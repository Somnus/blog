<p>给定一个二叉树，返回其节点值自底向上的层次遍历。 （即按从叶子节点所在层到根节点所在的层，逐层从左向右遍历）</p>

<p>例如：<br>
给定二叉树 <code>[3,9,20,null,null,15,7]</code>,</p>

<pre>    3
   / \
  9  20
    /  \
   15   7
</pre>

<p>返回其自底向上的层次遍历为：</p>

<pre>[
  [15,7],
  [9,20],
  [3]
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
    public IList<IList<int>> LevelOrderBottom(TreeNode root) {
        var list = new List<IList<int>>();      
        if(root == null) return list;

        Queue<TreeNode> queue = new Queue<TreeNode>();
        Stack<List<int>> stack =new Stack<List<int>>();
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
                stack.Push(midList);
                midList = new List<int>();
                count = queue.Count;
            }
        }

        while(stack.Count > 0){
            midList = stack.Pop();
            list.Add(midList);
        }

        return list;
    }
}
```