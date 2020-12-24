<p>给定一棵二叉搜索树，请找出其中第k大的节点。</p>

<p>&nbsp;</p>

<p><strong>示例 1:</strong></p>

<pre><strong>输入:</strong> root = [3,1,4,null,2], k = 1
   3
  / \
 1   4
  \
&nbsp;  2
<strong>输出:</strong> 4</pre>

<p><strong>示例 2:</strong></p>

<pre><strong>输入:</strong> root = [5,3,6,2,4,null,null,1], k = 3
       5
      / \
     3   6
    / \
   2   4
  /
 1
<strong>输出:</strong> 4</pre>

<p>&nbsp;</p>

<p><strong>限制：</strong></p>

<p>1 &le; k &le; 二叉搜索树元素个数</p>

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
    public int KthLargest(TreeNode root, int k) {
        var list = InorderTraversal(root);
        int result = list[list.Count - k];
        return result;
    }

    public IList<int> InorderTraversal(TreeNode root) {
        var list = new List<int>();
        if (root == null) return list;
        
        var stack = new Stack<TreeNode>();
        while (root != null || stack.Count > 0){
            /*指针root指向二叉树入栈，入栈后指针指向入栈二叉树的左子树*/
            while(root != null){
                stack.Push(root);
                root = root.left;
            }
            
            /*出栈，并将指针root指向出栈二叉树的右子树*/
            root = stack.Pop();
            list.Add(root.val);
            root = root.right;
        }
        return list;
    }
}
```