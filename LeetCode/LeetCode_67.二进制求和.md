<p>给你两个二进制字符串，返回它们的和（用二进制表示）。</p>

<p>输入为 <strong>非空 </strong>字符串且只包含数字&nbsp;<code>1</code>&nbsp;和&nbsp;<code>0</code>。</p>

<p>&nbsp;</p>

<p><strong>示例&nbsp;1:</strong></p>

<pre><strong>输入:</strong> a = &quot;11&quot;, b = &quot;1&quot;
<strong>输出:</strong> &quot;100&quot;</pre>

<p><strong>示例&nbsp;2:</strong></p>

<pre><strong>输入:</strong> a = &quot;1010&quot;, b = &quot;1011&quot;
<strong>输出:</strong> &quot;10101&quot;</pre>

<p>&nbsp;</p>

<p><strong>提示：</strong></p>

<ul>
	<li>每个字符串仅由字符 <code>&#39;0&#39;</code> 或 <code>&#39;1&#39;</code> 组成。</li>
	<li><code>1 &lt;= a.length, b.length &lt;= 10^4</code></li>
	<li>字符串如果不是 <code>&quot;0&quot;</code> ，就都不含前导零。</li>
</ul>

### C#代码

public class Solution {
    public string AddBinary(string a, string b) {
        int num1 = a.Length;
        int num2 = b.Length;
        int max = Math.Max(num1, num2);
        int min = Math.Min(num1, num2);
        string c = min == num1 ? a : b;
        string d = min == num1 ? b : a;

        Stack<int> stack = new Stack<int>();
        bool isAdd = false;
        int val;

        for (int i = min - 1; i > -1; i--)
        {
            var val1 = c[i];
            var val2 = d[i + max - min];
            val = val1 ^ val2;
            stack.Push(isAdd ? (val ^ 1) : val);
            isAdd = val1 == '1' && val2 == '1' || val == 1 && isAdd;
        }

        for (int i = max - min - 1; i > -1; i--)
        {
            int.TryParse(d[i].ToString(), out val);
            stack.Push(isAdd ? (val ^ 1) : val);
            isAdd = d[i] == '1' && isAdd;
        }

        if (isAdd)
            stack.Push(1);

        StringBuilder sb = new StringBuilder();
        while (stack.Count > 0)
            sb.Append(stack.Pop());

        return sb.ToString();
    }
}