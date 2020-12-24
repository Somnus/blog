<p>给你二叉搜索树的根节点 <code>root</code> ，同时给定最小边界<code>low</code> 和最大边界 <code>high</code>。通过修剪二叉搜索树，使得所有节点的值在<code>[low, high]</code>中。修剪树不应该改变保留在树中的元素的相对结构（即，如果没有被移除，原有的父代子代关系都应当保留）。 可以证明，存在唯一的答案。</p>

<p>所以结果应当返回修剪好的二叉搜索树的新的根节点。注意，根节点可能会根据给定的边界发生改变。</p>

<p> </p>

<p><strong>示例 1：</strong></p>
<img alt="" src="https://assets.leetcode.com/uploads/2020/09/09/trim1.jpg" style="width: 450px; height: 126px;" />
<pre>
<strong>输入：</strong>root = [1,0,2], low = 1, high = 2
<strong>输出：</strong>[1,null,2]
</pre>

<p><strong>示例 2：</strong></p>
<img alt="" src="https://assets.leetcode.com/uploads/2020/09/09/trim2.jpg" style="width: 450px; height: 277px;" />
<pre>
<strong>输入：</strong>root = [3,0,4,null,2,null,null,1], low = 1, high = 3
<strong>输出：</strong>[3,2,null,1]
</pre>

<p><strong>示例 3：</strong></p>

<pre>
<strong>输入：</strong>root = [1], low = 1, high = 2
<strong>输出：</strong>[1]
</pre>

<p><strong>示例 4：</strong></p>

<pre>
<strong>输入：</strong>root = [1,null,2], low = 1, high = 3
<strong>输出：</strong>[1,null,2]
</pre>

<p><strong>示例 5：</strong></p>

<pre>
<strong>输入：</strong>root = [1,null,2], low = 2, high = 4
<strong>输出：</strong>[2]
</pre>

<p> </p>

<p><strong>提示：</strong></p>

<ul>
	<li>树中节点数在范围 <code>[1, 10<sup>4</sup>]</code> 内</li>
	<li><code>0 <= Node.val <= 10<sup>4</sup></code></li>
	<li>树中每个节点的值都是唯一的</li>
	<li>题目数据保证输入是一棵有效的二叉搜索树</li>
	<li><code>0 <= low <= high <= 10<sup>4</sup></code></li>
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
    public TreeNode TrimBST(TreeNode root, int L, int R) {
        /*方案1*/
        //前序遍历每个节点，判断值是否在L与R之间。
        //若不在区间内，则需要选择一子节点代替当前节点。
        //场景1：左右孩子均存在：可选择左或右孩子替代当前节点。
        //场景2：只存在左孩子：左孩子替代当前节点。
        //场景3：只存在右孩子：右孩子替代当前节点。
        //场景4：不存在孩子：当前节点置空。
        
        /*方案2*/
        //当前节点值val,左边界L,右边界R
        //val<L:当前节点值小于L,那当前节点左子树上不存在节点其值在L和R之间，使用右孩子节点替代当前节点。
        //val>R:当前节点值大于R,那当前节点右子树上不存在节点其值在L和R之间，使用左孩子节点替代当前节点。
        //L<=val<=R:当前节点值在L和R之间。
        if (root == null) return null;

        if (root.val < L) return TrimBST(root.right, L,R);
        else if (root.val > R) return TrimBST(root.left, L, R);

        root.left = TrimBST(root.left, L, R);
        root.right = TrimBST(root.right, L, R);
        
        return root;
    }
}
```