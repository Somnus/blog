<p>实现一个二叉搜索树迭代器。你将使用二叉搜索树的根节点初始化迭代器。</p>

<p>调用 <code>next()</code> 将返回二叉搜索树中的下一个最小的数。</p>

<p>&nbsp;</p>

<p><strong>示例：</strong></p>

<p><strong><img alt="" src="https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/25/bst-tree.png" style="height: 178px; width: 189px;"></strong></p>

<pre>BSTIterator iterator = new BSTIterator(root);
iterator.next();    // 返回 3
iterator.next();    // 返回 7
iterator.hasNext(); // 返回 true
iterator.next();    // 返回 9
iterator.hasNext(); // 返回 true
iterator.next();    // 返回 15
iterator.hasNext(); // 返回 true
iterator.next();    // 返回 20
iterator.hasNext(); // 返回 false</pre>

<p>&nbsp;</p>

<p><strong>提示：</strong></p>

<ul>
	<li><code>next()</code>&nbsp;和&nbsp;<code>hasNext()</code>&nbsp;操作的时间复杂度是&nbsp;O(1)，并使用&nbsp;O(<em>h</em>) 内存，其中&nbsp;<em>h&nbsp;</em>是树的高度。</li>
	<li>你可以假设&nbsp;<code>next()</code>&nbsp;调用总是有效的，也就是说，当调用 <code>next()</code>&nbsp;时，BST 中至少存在一个下一个最小的数。</li>
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
public class BSTIterator {
    TreeNode node;
    IList<int> list = new List<int>();
    int index = 0;
    public BSTIterator(TreeNode root) {
        node = root;
        list = InorderTraversal(node);
    }
    
    /** @return the next smallest number */
    public int Next() {
        return list[index++];
    }
    
    /** @return whether we have a next smallest number */
    public bool HasNext() {
        return index < list.Count;
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

/**
 * Your BSTIterator object will be instantiated and called as such:
 * BSTIterator obj = new BSTIterator(root);
 * int param_1 = obj.Next();
 * bool param_2 = obj.HasNext();
 */
```