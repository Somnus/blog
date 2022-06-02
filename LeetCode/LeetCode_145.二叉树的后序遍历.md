<p>给定一个二叉树，返回它的 <em>后序&nbsp;</em>遍历。</p>

<p><strong>示例:</strong></p>

<pre><strong>输入:</strong> [1,null,2,3]  
   1
    \
     2
    /
   3 

<strong>输出:</strong> [3,2,1]</pre>

<p><strong>进阶:</strong>&nbsp;递归算法很简单，你可以通过迭代算法完成吗？</p>

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
    public IList<int> PostorderTraversal(TreeNode root) {
        var list = new List<int>();
        if (root == null) return list;

        TreeNode pre = null;
        var stack = new Stack<TreeNode>();
        stack.Push(root);
        /*当前节点被读取的条件为：无左右孩子，或者上一次读取的为其左孩子且右孩子为空，或者上一次读取的为其右孩子。*/
        while (stack.Count > 0)
        {
            TreeNode node = stack.Peek();
            if((node.left == null && node.right ==null) 
               || (pre != null && (pre == node.right || (pre == node.left && node.right == null)))){
                list.Add(node.val);
                stack.Pop();
                pre = node;
            }
            else{
                if(node.right != null) stack.Push(node.right);
                if(node.left != null) stack.Push(node.left);
            }
        }
        return list;
    }
}
```