<p>将两个升序链表合并为一个新的 <strong>升序</strong> 链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。&nbsp;</p>

<p>&nbsp;</p>

<p><strong>示例：</strong></p>

<pre><strong>输入：</strong>1-&gt;2-&gt;4, 1-&gt;3-&gt;4
<strong>输出：</strong>1-&gt;1-&gt;2-&gt;3-&gt;4-&gt;4
</pre>

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
    public ListNode MergeTwoLists(ListNode l1, ListNode l2) {
        if(l1==null)
        {
            return l2;
        }
        if(l2==null)
        {
            return l1;
        }

        Stack<int> stack = new Stack<int>();
        int val1,val2;

        while(l1!=null&& l2!=null)
        {
            val1=l1.val;
            val2=l2.val;
            if(val1<=val2)
            {
                stack.Push(val1);
                l1=l1.next;
            }
            else
            {
                stack.Push(val2);
                l2=l2.next;
            }
        }

        while(l1!=null)
        {
            val1=l1.val;
            stack.Push(val1);
            l1=l1.next;
        }

        while(l2!=null)
        {
            val2=l2.val;
            stack.Push(val2);
            l2=l2.next;
        }

        ListNode listNode = new ListNode(stack.Pop());
        while(stack.Count > 0)
        {
            listNode = new ListNode(stack.Pop())
            {
                next=listNode
            };
        }

        return listNode;
    }
}
```