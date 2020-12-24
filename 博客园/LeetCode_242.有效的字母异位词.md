<p>给定两个字符串 <em>s</em> 和 <em>t</em> ，编写一个函数来判断 <em>t</em> 是否是 <em>s</em> 的字母异位词。</p>

<p><strong>示例&nbsp;1:</strong></p>

<pre><strong>输入:</strong> <em>s</em> = &quot;anagram&quot;, <em>t</em> = &quot;nagaram&quot;
<strong>输出:</strong> true
</pre>

<p><strong>示例 2:</strong></p>

<pre><strong>输入:</strong> <em>s</em> = &quot;rat&quot;, <em>t</em> = &quot;car&quot;
<strong>输出: </strong>false</pre>

<p><strong>说明:</strong><br>
你可以假设字符串只包含小写字母。</p>

<p><strong>进阶:</strong><br>
如果输入字符串包含 unicode 字符怎么办？你能否调整你的解法来应对这种情况？</p>

### C#代码

```
public class Solution {
    public bool IsAnagram(string s, string t) {
        Dictionary<char, int> dic = new Dictionary<char, int>();
        foreach (var item in s)
        {
            int num;
            if (dic.TryGetValue(item, out num))
            {
                dic.Remove(item);
                num += 1;
            }
            else
            {
                num = 1;
            }
            dic.Add(item, num);
        }

        foreach (var item in t)
        {
            if (dic.TryGetValue(item, out int num))
            {
                dic.Remove(item);
                if (num > 1)
                {
                    dic.Add(item, num - 1);
                }
            }
            else
            {
                return false;
            }
        }

        if (dic.Count > 0)
        {
            return false;
        }
        return true;
    }
}
```