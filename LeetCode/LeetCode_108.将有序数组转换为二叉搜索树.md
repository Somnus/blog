<p>将一个按照升序排列的有序数组，转换为一棵高度平衡二叉搜索树。</p>

<p>本题中，一个高度平衡二叉树是指一个二叉树<em>每个节点&nbsp;</em>的左右两个子树的高度差的绝对值不超过 1。</p>

<p><strong>示例:</strong></p>

<pre>给定有序数组: [-10,-3,0,5,9],

一个可能的答案是：[0,-3,9,-10,null,5]，它可以表示下面这个高度平衡二叉搜索树：

      0
     / \
   -3   9
   /   /
 -10  5
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
    public TreeNode SortedArrayToBST(int[] nums) {
        return SortedArrayToBST(nums, 0, nums.Length - 1);
    }

    public TreeNode SortedArrayToBST(int[] nums, int left, int right){
        if(nums.Length == 0)
            return null;

        int index = ( left + right) / 2;
        int val = nums[index];
        TreeNode tree = new TreeNode(val);

        if(left <= index - 1)
            tree.left = SortedArrayToBST(nums, left, index - 1);

        if(right >= index + 1)
            tree.right = SortedArrayToBST(nums, index + 1, right);

        return tree;
    }
}
```