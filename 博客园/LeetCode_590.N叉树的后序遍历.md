<p>给定一个 N 叉树，返回其节点值的<em>后序遍历</em>。</p>

<p>例如，给定一个&nbsp;<code>3叉树</code>&nbsp;:</p>

<p>&nbsp;</p>

<p><img src="https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/10/12/narytreeexample.png" style="width: 100%; max-width: 300px;"></p>

<p>&nbsp;</p>

<p>返回其后序遍历: <code>[5,6,3,2,4,1]</code>.</p>

<p>&nbsp;</p>

<p><strong>说明:</strong>&nbsp;递归法很简单，你可以使用迭代法完成此题吗?</p>
### C#代码

```
/*
// Definition for a Node.
public class Node {
    public int val;
    public IList<Node> children;

    public Node() {}

    public Node(int _val) {
        val = _val;
    }

    public Node(int _val, IList<Node> _children) {
        val = _val;
        children = _children;
    }
}
*/
public class Solution {
    private IList<int> list = new List<int>();
    public IList<int> Postorder(Node root) {
        if(root != null){
            if(root.children.Any()){
                foreach(var item in root.children){
                    Postorder(item);
                }
            }
            list.Add(root.val);
        }
        return list;
    }
}
```