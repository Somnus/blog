<p>给出一个<strong>完全二叉树</strong>，求出该树的节点个数。</p>

<p><strong>说明：</strong></p>

<p><a href="https://baike.baidu.com/item/%E5%AE%8C%E5%85%A8%E4%BA%8C%E5%8F%89%E6%A0%91/7773232?fr=aladdin">完全二叉树</a>的定义如下：在完全二叉树中，除了最底层节点可能没填满外，其余每层节点数都达到最大值，并且最下面一层的节点都集中在该层最左边的若干位置。若最底层为第 h 层，则该层包含 1~&nbsp;2<sup>h</sup>&nbsp;个节点。</p>

<p><strong>示例:</strong></p>

<pre><strong>输入:</strong> 
    1
   / \
  2   3
 / \  /
4  5 6

<strong>输出:</strong> 6</pre>

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
    /*深度字典，用于减少深度遍历次数。*/
    private Dictionary<TreeNode,int> dic = new Dictionary<TreeNode,int>();
    private int NodeLevel(TreeNode root){
        if(root == null) return 0;
        if(dic.ContainsKey(root)) return dic[root];
        
        int level = NodeLevel(root.left) + 1;
        dic.Add(root, level);
        
        return level;    
    }
    public int CountNodes(TreeNode root) {
        if(root == null) return 0;
        
        /*计算左右子树的深度*/
        int leftLevel = NodeLevel(root.left);
        int rightLevel = NodeLevel(root.right);

        /*左右子树高度谁高谁为满二叉树，另一子树未完全二叉树。高度h的满二叉树，节点总数为2^h-1；*/
        if(leftLevel == rightLevel) return (1 << leftLevel) + CountNodes(root.right);
        return (1 << rightLevel) + CountNodes(root.left);
    }
}
```