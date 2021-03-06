<p>根据一棵树的中序遍历与后序遍历构造二叉树。</p>

<p><strong>注意:</strong><br>
你可以假设树中没有重复的元素。</p>

<p>例如，给出</p>

<pre>中序遍历 inorder =&nbsp;[9,3,15,20,7]
后序遍历 postorder = [9,15,7,20,3]</pre>

<p>返回如下的二叉树：</p>

<pre>    3
   / \
  9  20
    /  \
   15   7
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
    public TreeNode BuildTree(int[] inorder, int[] postorder) {
        int index = postorder.Length - 1;
        return BuildTree(postorder, inorder, 0, postorder.Length - 1,ref index);
    }
    public TreeNode BuildTree(int[] postorder, int[] inorder, int left, int right, ref int index)
    {
        /*递归边界条件：1.两数组长度不一致；2.任意数组长度为0.*/
        if (postorder.Length != inorder.Length || postorder.Length == 0) return null;

        TreeNode node = new TreeNode(postorder[index]);//current node

        /*找到当前节点值在inorder中索引*/
        int l = left;
        while (l >= left && l <= right)
        {
            if (inorder[l] == postorder[index]) break;
            l++;
        }

        index -= 1;
        if (right - l > 0) node.right = BuildTree(postorder, inorder, l + 1, right, ref index);
        if (l - left > 0) node.left = BuildTree(postorder, inorder, left, l - 1, ref index);

        return node;
    }
}
```