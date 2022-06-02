<p>输入某二叉树的前序遍历和中序遍历的结果，请重建该二叉树。假设输入的前序遍历和中序遍历的结果中都不含重复的数字。</p>

<p>&nbsp;</p>

<p>例如，给出</p>

<pre>前序遍历 preorder =&nbsp;[3,9,20,15,7]
中序遍历 inorder = [9,3,15,20,7]</pre>

<p>返回如下的二叉树：</p>

<pre>    3
   / \
  9  20
    /  \
   15   7</pre>

<p>&nbsp;</p>

<p><strong>限制：</strong></p>

<p><code>0 &lt;= 节点个数 &lt;= 5000</code></p>

<p>&nbsp;</p>

<p><strong>注意</strong>：本题与主站 105 题重复：<a href="https://leetcode-cn.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/">https://leetcode-cn.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/</a></p>

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
    public TreeNode BuildTree(int[] preorder, int[] inorder) {
        if(preorder.Length == 0 || inorder.Length == 0 || preorder.Length != inorder.Length) return null;
        int preIndex = 0;
        return BuildTree(preorder, inorder, 0, inorder.Length - 1, ref preIndex);
    }

    public TreeNode BuildTree(int[] preorder, int[] inorder, int inLeft, int inRight, ref int preIndex)
    {
        //if (preorder.Length != inorder.Length || preorder.Length == 0) return null;

        int val = preorder[preIndex++];
        TreeNode treeNode = new TreeNode(val);

        int l = inLeft;
        while(l < inRight){
            if(inorder[l] == val) break;
            l++;
        }

        if(l - inLeft > 0) treeNode.left = BuildTree(preorder, inorder, inLeft, l - 1, ref preIndex);
        if(inRight - l > 0) treeNode.right = BuildTree(preorder, inorder, l + 1, inRight, ref preIndex);

        return treeNode;
    }
}
```