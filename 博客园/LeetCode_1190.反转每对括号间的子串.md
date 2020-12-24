<p>给出一个字符串&nbsp;<code>s</code>（仅含有小写英文字母和括号）。</p>

<p>请你按照从括号内到外的顺序，逐层反转每对匹配括号中的字符串，并返回最终的结果。</p>

<p>注意，您的结果中 <strong>不应</strong> 包含任何括号。</p>

<p>&nbsp;</p>

<p><strong>示例 1：</strong></p>

<pre><strong>输入：</strong>s = &quot;(abcd)&quot;
<strong>输出：</strong>&quot;dcba&quot;
</pre>

<p><strong>示例 2：</strong></p>

<pre><strong>输入：</strong>s = &quot;(u(love)i)&quot;
<strong>输出：</strong>&quot;iloveu&quot;
</pre>

<p><strong>示例 3：</strong></p>

<pre><strong>输入：</strong>s = &quot;(ed(et(oc))el)&quot;
<strong>输出：</strong>&quot;leetcode&quot;
</pre>

<p><strong>示例 4：</strong></p>

<pre><strong>输入：</strong>s = &quot;a(bcdefghijkl(mno)p)q&quot;
<strong>输出：</strong>&quot;apmnolkjihgfedcbq&quot;
</pre>

<p>&nbsp;</p>

<p><strong>提示：</strong></p>

<ul>
	<li><code>0 &lt;= s.length &lt;= 2000</code></li>
	<li><code>s</code> 中只有小写英文字母和括号</li>
	<li>我们确保所有括号都是成对出现的</li>
</ul>

### C#代码

```
public class Solution {
    public string ReverseParentheses(string str) {
        Stack<int> stack = new Stack<int>();
        for (int i = 0; i < str.Length; i++)
        {
            if (str[i] == '(')
            {
                stack.Push(i);
            }
            else if (str[i] == ')')
            {
                var startIndex = stack.Pop();
                string mid = str.Substring(startIndex + 1, i - 1 - startIndex);
                mid = new string(mid.Reverse().ToArray());

                string startStr = str.Substring(0, startIndex);
                string endStr = str.Substring(i + 1, str.Length - 1 - i);

                str = startStr + "_" + mid + "_" + endStr;
            }
        }
        return new string(str.Where(p=>p!='_').ToArray());        
    }
}
```