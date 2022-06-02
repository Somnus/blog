<p>请实现一个函数，把字符串 <code>s</code> 中的每个空格替换成&quot;%20&quot;。</p>

<p>&nbsp;</p>

<p><strong>示例 1：</strong></p>

<pre><strong>输入：</strong>s = &quot;We are happy.&quot;
<strong>输出：</strong>&quot;We%20are%20happy.&quot;</pre>

<p>&nbsp;</p>

<p><strong>限制：</strong></p>

<p><code>0 &lt;= s 的长度 &lt;= 10000</code></p>

### C#代码

```
public class Solution {
    public string ReplaceSpace(string s) {
        StringBuilder sb=new StringBuilder();
        for(int i=0;i<s.Length;i++){
            if(s[i]==' '){
                sb.Append("%20");
            }
            else{
                sb.Append(s[i]);
            }
        }
        return sb.ToString();
    }
}
```