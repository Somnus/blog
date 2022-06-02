<p>给你两个 <strong>非空 </strong>链表来代表两个非负整数。数字最高位位于链表开始位置。它们的每个节点只存储一位数字。将这两数相加会返回一个新的链表。</p>

<p>你可以假设除了数字 0 之外，这两个数字都不会以零开头。</p>

<p>&nbsp;</p>

<p><strong>进阶：</strong></p>

<p>如果输入链表不能修改该如何处理？换句话说，你不能对列表中的节点进行翻转。</p>

<p>&nbsp;</p>

<p><strong>示例：</strong></p>

<pre><strong>输入：</strong>(7 -&gt; 2 -&gt; 4 -&gt; 3) + (5 -&gt; 6 -&gt; 4)
<strong>输出：</strong>7 -&gt; 8 -&gt; 0 -&gt; 7
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
    public ListNode AddTwoNumbers(ListNode l1, ListNode l2) {
        Stack<int> stack1 = new Stack<int>();
        Stack<int> stack2 = new Stack<int>();
        while(l1 != null){
            stack1.Push(l1.val);
            l1=l1.next;
        }
        while(l2 != null){
            stack2.Push(l2.val);
            l2=l2.next;
        }

        int max = Math.Max(stack1.Count, stack2.Count);
        int val1,val2,sum,val,diff;
        diff = 0;
        ListNode next = null;
        for(int i = 0; i < max; i++){
            val1 = stack1.Count > 0 ? stack1.Pop() : 0;
            val2 = stack2.Count > 0 ? stack2.Pop() : 0;
            sum = val1 + val2 + diff;
            val = sum % 10;
            diff =sum / 10;

            ListNode node = new ListNode(val);
            node.next = next;
            next = node;

            if(i == max-1 && diff > 0){
                max += 1;
            }
        }

        return next;
    }
}
```