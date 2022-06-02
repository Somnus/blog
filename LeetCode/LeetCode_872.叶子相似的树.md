<p>请考虑一棵二叉树上所有的叶子，这些叶子的值按从左到右的顺序排列形成一个&nbsp;<em>叶值序列</em> 。</p>

<p><img alt="" src="https://s3-lc-upload.s3.amazonaws.com/uploads/2018/07/16/tree.png" style="height: 240px; width: 300px;"></p>

<p>举个例子，如上图所示，给定一棵叶值序列为&nbsp;<code>(6, 7, 4, 9, 8)</code>&nbsp;的树。</p>

<p>如果有两棵二叉树的叶值序列是相同，那么我们就认为它们是&nbsp;<em>叶相似&nbsp;</em>的。</p>

<p>如果给定的两个头结点分别为&nbsp;<code>root1</code> 和&nbsp;<code>root2</code>&nbsp;的树是叶相似的，则返回&nbsp;<code>true</code>；否则返回 <code>false</code> 。</p>

<p>&nbsp;</p>

<p><strong>示例 1：</strong></p>

<p><img alt="" src="https://assets.leetcode.com/uploads/2020/09/03/leaf-similar-1.jpg" style="height: 297px; width: 750px;"></p>

<pre><strong>输入：</strong>root1 = [3,5,1,6,2,9,8,null,null,7,4], root2 = [3,5,1,6,7,4,2,null,null,null,null,null,null,9,8]
<strong>输出：</strong>true
</pre>

<p><strong>示例 2：</strong></p>

<pre><strong>输入：</strong>root1 = [1], root2 = [1]
<strong>输出：</strong>true
</pre>

<p><strong>示例 3：</strong></p>

<pre><strong>输入：</strong>root1 = [1], root2 = [2]
<strong>输出：</strong>false
</pre>

<p><strong>示例 4：</strong></p>

<pre><strong>输入：</strong>root1 = [1,2], root2 = [2,2]
<strong>输出：</strong>true
</pre>

<p><strong>示例 5：</strong></p>

<p><img alt="" src="https://assets.leetcode.com/uploads/2020/09/03/leaf-similar-2.jpg" style="height: 165px; width: 450px;"></p>

<pre><strong>输入：</strong>root1 = [1,2,3], root2 = [1,3,2]
<strong>输出：</strong>false
</pre>

<p>&nbsp;</p>

<p><strong>提示：</strong></p>

<ul>
	<li>给定的两棵树可能会有&nbsp;<code>1</code>&nbsp;到 <code>200</code>&nbsp;个结点。</li>
	<li>给定的两棵树上的值介于 <code>0</code> 到 <code>200</code> 之间。</li>
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
    public bool LeafSimilar(TreeNode root1, TreeNode root2) {
        var list1 = LeafSimilar(root1);
        var list2 = LeafSimilar(root2);

        if(list1.Count != list2.Count) return false;

        for(int i = 0; i < list1.Count; i++){
            if(list1[i] != list2[i]) return false;
        }
        return true;
    }

    private List<int> LeafSimilar(TreeNode root){
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
            if(root.left == null && root.right == null) list.Add(root.val);
            root = root.right;
        }
        return list;
    }
}
```