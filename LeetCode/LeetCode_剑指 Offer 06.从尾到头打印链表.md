<p>输入一个链表的头节点，从尾到头反过来返回每个节点的值（用数组返回）。</p>

<p>&nbsp;</p>

<p><strong>示例 1：</strong></p>

<pre><strong>输入：</strong>head = [1,3,2]
<strong>输出：</strong>[2,3,1]</pre>

<p>&nbsp;</p>

<p><strong>限制：</strong></p>

<p><code>0 &lt;= 链表长度 &lt;= 10000</code></p>

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
    private Stack<int> stack = new Stack<int>();
    public int[] ReversePrint(ListNode head)
    {
        Set(head);

        int[] array = new int[stack.Count];

        for (int i = 0; i < array.Length; i++)
            array[i] = stack.Pop();

        return array;
    }
    private void Set(ListNode head)
    {
        if (head == null)
            return;

        stack.Push(head.val);

        if (head.next != null)
            Set(head.next);
    }
}
```