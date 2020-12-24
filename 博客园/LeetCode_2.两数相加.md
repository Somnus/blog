<p>给出两个&nbsp;<strong>非空</strong> 的链表用来表示两个非负的整数。其中，它们各自的位数是按照&nbsp;<strong>逆序</strong>&nbsp;的方式存储的，并且它们的每个节点只能存储&nbsp;<strong>一位</strong>&nbsp;数字。</p>

<p>如果，我们将这两个数相加起来，则会返回一个新的链表来表示它们的和。</p>

<p>您可以假设除了数字 0 之外，这两个数都不会以 0&nbsp;开头。</p>

<p><strong>示例：</strong></p>

<pre><strong>输入：</strong>(2 -&gt; 4 -&gt; 3) + (5 -&gt; 6 -&gt; 4)
<strong>输出：</strong>7 -&gt; 0 -&gt; 8
<strong>原因：</strong>342 + 465 = 807
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
    public class Solution 
    {      
        /// <summary>
        /// 计算两个链表相加结果：递归法
        /// </summary>
        /// <param name="l1">链表1</param>
        /// <param name="l2">链表2</param>
        /// <param name="num">进位<param>
        /// <returns>相加结果逆序链表</returns>
        public ListNode AddTwoNumbers(ListNode l1, ListNode l2, int num = 0)
        {
            /*递归边界：两个链表均为空*/
            if (l1 == null && l2 == null)
            {
                if(num != 0 ) return new ListNode(num);
                else return default;
            }

            /*计算两链表当前节点值相加之和：节点1值+节点2值+进位值*/ 
            int midValue = (l1?.val ?? 0) + (l2?.val ?? 0) + num;        

            /*创建节点储存相加之和非进位部分*/
            ListNode listNode = new ListNode(midValue % 10);

            /*两链表指针右移，继续计算。*/
            listNode.next = AddTwoNumbers(l1?.next, l2?.next, midValue / 10);

            return listNode;
        }

        // /// <summary>
        // /// 计算两个链表相加结果:迭代法
        // /// </summary>
        // /// <param name="l1">链表1</param>
        // /// <param name="l2">链表2</param>
        // /// <param name="num">进位<param>
        // /// <returns>相加结果逆序链表</returns>
        // public ListNode AddTwoNumbers(ListNode l1, ListNode l2)
        // {
        //     if(l1 == null) return l2;
        //     if(l2 == null) return l1;

        //     ListNode listNode = new ListNode(0);
        //     ListNode current = listNode;
        //     int num = 0;
        //     int val;

        //     while(l1 != null || l2 != null || num != 0)
        //     {
        //         /*计算两链表当前节点值相加之和：节点1值+节点2值+进位值*/ 
        //         val = (l1?.val ?? 0) + (l2?.val ?? 0) + num;
        //         /*更新更新子节点*/
        //         current.next = new ListNode(val % 10);

        //         /*更新进位值*/
        //         num = val / 10;

        //         /*l1、l2、current指针均右移*/
        //         if(l1 != null) l1 = l1.next;
        //         if(l2 != null) l2 = l2.next;
        //         current = current.next;
        //     }
        //     return listNode.next;
        // }

    }
```