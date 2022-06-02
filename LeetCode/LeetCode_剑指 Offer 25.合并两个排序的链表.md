<p>输入两个递增排序的链表，合并这两个链表并使新链表中的节点仍然是递增排序的。</p>

<p><strong>示例1：</strong></p>

<pre><strong>输入：</strong>1-&gt;2-&gt;4, 1-&gt;3-&gt;4
<strong>输出：</strong>1-&gt;1-&gt;2-&gt;3-&gt;4-&gt;4</pre>

<p><strong>限制：</strong></p>

<p><code>0 &lt;= 链表长度 &lt;= 1000</code></p>

<p>注意：本题与主站 21 题相同：<a href="https://leetcode-cn.com/problems/merge-two-sorted-lists/">https://leetcode-cn.com/problems/merge-two-sorted-lists/</a></p>

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
        if(l1 == null) return l2;
        if(l2 == null) return l1;
        if(l1.val < l2.val){
            l1.next = MergeTwoLists(l1.next, l2);
            return l1;
        }
        
        l2.next = MergeTwoLists(l1, l2.next);
        return l2;
    }
}
```