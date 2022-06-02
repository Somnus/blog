<p>请判断一个链表是否为回文链表。</p>

<p><strong>示例 1:</strong></p>

<pre><strong>输入:</strong> 1-&gt;2
<strong>输出:</strong> false</pre>

<p><strong>示例 2:</strong></p>

<pre><strong>输入:</strong> 1-&gt;2-&gt;2-&gt;1
<strong>输出:</strong> true
</pre>

<p><strong>进阶：</strong><br>
你能否用&nbsp;O(n) 时间复杂度和 O(1) 空间复杂度解决此题？</p>

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
    public bool IsPalindrome(ListNode head) 
    {
        if(head==null){
            return true;
        }

        List<int> list = new List<int>();
        while(head!=null)
        {
            list.Add( head.val);
            head = head.next;
        }

        int count = list.Count;
        int mid = count / 2;
        for(int i = 0; i < mid ; i++)
        {
            if(list[i]!=list[count-i-1])
            {
                return false;
            }
        }
        return true;
    }
}
```