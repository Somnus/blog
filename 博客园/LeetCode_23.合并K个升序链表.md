<p>给你一个链表数组，每个链表都已经按升序排列。</p>

<p>请你将所有链表合并到一个升序链表中，返回合并后的链表。</p>

<p>&nbsp;</p>

<p><strong>示例 1：</strong></p>

<pre><strong>输入：</strong>lists = [[1,4,5],[1,3,4],[2,6]]
<strong>输出：</strong>[1,1,2,3,4,4,5,6]
<strong>解释：</strong>链表数组如下：
[
  1-&gt;4-&gt;5,
  1-&gt;3-&gt;4,
  2-&gt;6
]
将它们合并到一个有序链表中得到。
1-&gt;1-&gt;2-&gt;3-&gt;4-&gt;4-&gt;5-&gt;6
</pre>

<p><strong>示例 2：</strong></p>

<pre><strong>输入：</strong>lists = []
<strong>输出：</strong>[]
</pre>

<p><strong>示例 3：</strong></p>

<pre><strong>输入：</strong>lists = [[]]
<strong>输出：</strong>[]
</pre>

<p>&nbsp;</p>

<p><strong>提示：</strong></p>

<ul>
	<li><code>k == lists.length</code></li>
	<li><code>0 &lt;= k &lt;= 10^4</code></li>
	<li><code>0 &lt;= lists[i].length &lt;= 500</code></li>
	<li><code>-10^4 &lt;= lists[i][j] &lt;= 10^4</code></li>
	<li><code>lists[i]</code> 按 <strong>升序</strong> 排列</li>
	<li><code>lists[i].length</code> 的总和不超过 <code>10^4</code></li>
</ul>

### C#代码

```
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     public int val;
 *     public ListNode next;
 *     public ListNode(int x) { val = x; }
 * }
 */
public class Solution {
    public ListNode MergeKLists(ListNode[] lists)
    {
        int index = 0;
        int count = lists.Length;
        int temp = 1;

        if (count == 0) return null;

        while (temp < count)
        {
            index = 0;
            while (index + temp < count)
            {
                lists[index] = MergeTwoList(lists[index], lists[index + temp]);
                index += temp * 2;
            }
            temp *= 2;
        }
        return lists[0];
    }

    public ListNode MergeTwoList(ListNode node1, ListNode node2)
    {
        Stack<int> stack = new Stack<int>();
        int val1, val2;
        while (node1 != null && node2 != null)
        {
            val1 = node1.val;
            val2 = node2.val;
            if (val1 <= val2)
            {
                stack.Push(val1);
                node1 = node1.next;
            }
            else
            {
                stack.Push(val2);
                node2 = node2.next;
            }
        }

        while (node1 != null)
        {
            val1 = node1.val;
            stack.Push(val1);
            node1 = node1.next;
        }

        while (node2 != null)
        {
            val2 = node2.val;
            stack.Push(val2);
            node2 = node2.next;
        }

        ListNode node = null;
        while (stack.Count > 0)
        {
            node = new ListNode() { val = stack.Pop(), next = node };
        }
        return node;
    }
}
```