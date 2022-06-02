<p>输入一个链表，输出该链表中倒数第k个节点。为了符合大多数人的习惯，本题从1开始计数，即链表的尾节点是倒数第1个节点。例如，一个链表有6个节点，从头节点开始，它们的值依次是1、2、3、4、5、6。这个链表的倒数第3个节点是值为4的节点。</p>

<p>&nbsp;</p>

<p><strong>示例：</strong></p>

<pre>给定一个链表: <strong>1-&gt;2-&gt;3-&gt;4-&gt;5</strong>, 和 <em>k </em><strong>= 2</strong>.

返回链表 4<strong>-&gt;5</strong>.</pre>

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
    public ListNode GetKthFromEnd(ListNode head, int k) {
        if(head == null) return head;
        
        Dictionary<int,ListNode> dic = new Dictionary<int,ListNode>();
        while(head != null){
            dic.Add(dic.Count + 1,head);
            head = head.next;
        }
        
        if(dic.Count < k) return null;
        return dic[dic.Count - k + 1];
    }
}
```