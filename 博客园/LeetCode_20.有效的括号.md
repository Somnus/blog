<p>给定一个只包括 <code>&#39;(&#39;</code>，<code>&#39;)&#39;</code>，<code>&#39;{&#39;</code>，<code>&#39;}&#39;</code>，<code>&#39;[&#39;</code>，<code>&#39;]&#39;</code>&nbsp;的字符串，判断字符串是否有效。</p>

<p>有效字符串需满足：</p>

<ol>
	<li>左括号必须用相同类型的右括号闭合。</li>
	<li>左括号必须以正确的顺序闭合。</li>
</ol>

<p>注意空字符串可被认为是有效字符串。</p>

<p><strong>示例 1:</strong></p>

<pre><strong>输入:</strong> &quot;()&quot;
<strong>输出:</strong> true
</pre>

<p><strong>示例&nbsp;2:</strong></p>

<pre><strong>输入:</strong> &quot;()[]{}&quot;
<strong>输出:</strong> true
</pre>

<p><strong>示例&nbsp;3:</strong></p>

<pre><strong>输入:</strong> &quot;(]&quot;
<strong>输出:</strong> false
</pre>

<p><strong>示例&nbsp;4:</strong></p>

<pre><strong>输入:</strong> &quot;([)]&quot;
<strong>输出:</strong> false
</pre>

<p><strong>示例&nbsp;5:</strong></p>

<pre><strong>输入:</strong> &quot;{[]}&quot;
<strong>输出:</strong> true</pre>

### C#代码

```
public class Solution {
    public bool IsValid(string s) {
        List<char> list1 = new List<char>() { '[', '{', '(' };
        List<char> list2 = new List<char>() { ']', '}', ')' };
        Stack<char> stack = new Stack<char>();
        for (int i = 0; i < s.Length; i++)
        {
            if (list1.Contains(s[i]))
                stack.Push(s[i]);

            if (list2.Contains(s[i]))
            {
                if (stack.Count == 0)
                    return false;
                
                stack.TryPeek(out char str);

                if ((str == '(' && s[i] != ')') || (str == '[' && s[i] != ']') || (str == '{' && s[i] != '}'))
                    return false;
                
                stack.TryPop(out char str1);
            }
        }
        return stack.Count == 0;
    }
}
```