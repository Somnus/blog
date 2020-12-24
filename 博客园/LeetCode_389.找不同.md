<p>给定两个字符串 <em><strong>s</strong></em> 和 <em><strong>t</strong></em>，它们只包含小写字母。</p>

<p>字符串&nbsp;<strong><em>t</em></strong>&nbsp;由字符串&nbsp;<strong><em>s</em></strong>&nbsp;随机重排，然后在随机位置添加一个字母。</p>

<p>请找出在 <em><strong>t</strong></em> 中被添加的字母。</p>

<p>&nbsp;</p>

<p><strong>示例 1：</strong></p>

<pre><strong>输入：</strong>s = &quot;abcd&quot;, t = &quot;abcde&quot;
<strong>输出：</strong>&quot;e&quot;
<strong>解释：</strong>&#39;e&#39; 是那个被添加的字母。
</pre>

<p><strong>示例 2：</strong></p>

<pre><strong>输入：</strong>s = &quot;&quot;, t = &quot;y&quot;
<strong>输出：</strong>&quot;y&quot;
</pre>

<p><strong>示例 3：</strong></p>

<pre><strong>输入：</strong>s = &quot;a&quot;, t = &quot;aa&quot;
<strong>输出：</strong>&quot;a&quot;
</pre>

<p><strong>示例 4：</strong></p>

<pre><strong>输入：</strong>s = &quot;ae&quot;, t = &quot;aea&quot;
<strong>输出：</strong>&quot;a&quot;
</pre>

<p>&nbsp;</p>

<p><strong>提示：</strong></p>

<ul>
	<li><code>0 &lt;= s.length &lt;= 1000</code></li>
	<li><code>t.length == s.length + 1</code></li>
	<li><code>s</code> 和 <code>t</code> 只包含小写字母</li>
</ul>

### C#代码

```
public class Solution {
    public char FindTheDifference(string s, string t) {
        Dictionary<char,int> dic =new Dictionary<char,int>();
        for(int i = 0; i< s.Length; i++){
            if(dic.ContainsKey(s[i])) dic[s[i]] += 1;
            else dic.Add(s[i], 1);
        }

        for(int i = 0; i < t.Length; i++){
            if(!dic.ContainsKey(t[i])) return t[i];
            else if(dic[t[i]] == 1) dic.Remove(t[i]);
            else dic[t[i]] -= 1;
        }

        return ' ';
    }
}
```