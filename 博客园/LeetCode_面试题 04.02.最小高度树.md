<p>给定一个有序整数数组，元素各不相同且按升序排列，编写一个算法，创建一棵高度最小的二叉搜索树。</p><strong>示例:</strong><pre>给定有序数组: [-10,-3,0,5,9],<br><br>一个可能的答案是：[0,-3,9,-10,null,5]，它可以表示下面这个高度平衡二叉搜索树：<br><br>          0 <br>         / &#92 <br>       -3   9 <br>       /   / <br>     -10  5 <br></pre>
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
    public TreeNode SortedArrayToBST(int[] nums, int left = -1, int right = -1) {
       /*满二叉树同样深度中结点个数最多，不可达则构建完全二叉树，则二分法即可*/ 
        if (nums.Length == 0) return null;
        if (left == -1) left = 0;
        if (right == -1) right = nums.Length - 1;

        int mid = (left + right) / 2;
        TreeNode node = new TreeNode(nums[mid]);

        if (mid > left) node.left = SortedArrayToBST(nums, left, mid - 1);
        if (mid < right) node.right = SortedArrayToBST(nums, mid + 1, right);

        return node;
    }
}
```