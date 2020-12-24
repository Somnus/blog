<p>给定一个仅包含大小写字母和空格&nbsp;<code>&#39; &#39;</code>&nbsp;的字符串 <code>s</code>，返回其最后一个单词的长度。如果字符串从左向右滚动显示，那么最后一个单词就是最后出现的单词。</p>

<p>如果不存在最后一个单词，请返回 0&nbsp;。</p>

<p><strong>说明：</strong>一个单词是指仅由字母组成、不包含任何空格字符的 <strong>最大子字符串</strong>。</p>

<p>&nbsp;</p>

<p><strong>示例:</strong></p>

<pre><strong>输入:</strong> &quot;Hello World&quot;
<strong>输出:</strong> 5
</pre>

### C#代码

```
public class Solution {
    public int LengthOfLastWord(string s) {
        int num = 0;
        for(int i = s.Length-1; i > -1; i--)
        {
            if(num != 0 && s[i] == ' ')
                return num;
                
            if(s[i] != ' ')
                num++;
        }
        return num;
    }
}
```