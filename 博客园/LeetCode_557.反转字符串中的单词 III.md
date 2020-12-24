<p>给定一个字符串，你需要反转字符串中每个单词的字符顺序，同时仍保留空格和单词的初始顺序。</p>

<p>&nbsp;</p>

<p><strong>示例：</strong></p>

<pre><strong>输入：</strong>&quot;Let&#39;s take LeetCode contest&quot;
<strong>输出：</strong>&quot;s&#39;teL ekat edoCteeL tsetnoc&quot;
</pre>

<p>&nbsp;</p>

<p><strong><strong><strong><strong>提示：</strong></strong></strong></strong></p>

<ul>
	<li>在字符串中，每个单词由单个空格分隔，并且字符串中不会有任何额外的空格。</li>
</ul>

### C#代码

```
public class Solution {
    public string ReverseWords(string s)
    {
        StringBuilder builder = new StringBuilder();
        string[] midStrList = s.Split(' ');
        Stack<char> strStack = new Stack<char>();

        for (int i = 0; i < midStrList.Length; i++)
        {
            string midStr = midStrList[i];
            for (int j = 0; j < midStr.Length; j++)
            {
                strStack.Push(midStr[j]);
            }

            while (strStack.Any())
            {
                builder.Append(strStack.Pop());
            }

            if (i < midStrList.Length - 1)
            {
                builder.Append(' ');
            }
        }

        return builder.ToString();
    }
}
```