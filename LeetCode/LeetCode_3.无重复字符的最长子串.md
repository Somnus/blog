<p>给定一个字符串，请你找出其中不含有重复字符的 <strong>最长子串 </strong>的长度。</p>

<p> </p>

<p><strong>示例 1:</strong></p>

<pre>
<strong>输入: </strong>s = "abcabcbb"
<strong>输出: </strong>3 
<strong>解释:</strong> 因为无重复字符的最长子串是 <code>"abc"，所以其</code>长度为 3。
</pre>

<p><strong>示例 2:</strong></p>

<pre>
<strong>输入: </strong>s = "bbbbb"
<strong>输出: </strong>1
<strong>解释: </strong>因为无重复字符的最长子串是 <code>"b"</code>，所以其长度为 1。
</pre>

<p><strong>示例 3:</strong></p>

<pre>
<strong>输入: </strong>s = "pwwkew"
<strong>输出: </strong>3
<strong>解释: </strong>因为无重复字符的最长子串是 <code>"wke"</code>，所以其长度为 3。
     请注意，你的答案必须是 <strong>子串 </strong>的长度，<code>"pwke"</code> 是一个<em>子序列，</em>不是子串。
</pre>

<p><strong>示例 4:</strong></p>

<pre>
<strong>输入: </strong>s = ""
<strong>输出: </strong>0
</pre>

<p> </p>

<p><strong>提示：</strong></p>

<ul>
	<li><code>0 <= s.length <= 5 * 10<sup>4</sup></code></li>
	<li><code>s</code> 由英文字母、数字、符号和空格组成</li>
</ul>

### C#代码

```
public class Solution {
    public int LengthOfLongestSubstring(string s) {
        int max = 0;
        int index = -1;
        int temp = -1;
        Dictionary<char,int> dic = new Dictionary<char,int>();
        for(int i = 0; i < s.Length; i++){
            if(dic.ContainsKey(s[i])){
                if(s[i] == s[i - 1]) dic.Clear();
                else{
                    temp =dic[s[i]];
                    for(int j = temp; j > index; j--){
                        dic.Remove(s[j]);
                    }
                    index = temp;
                }
            }

            dic.Add(s[i],i);
            if(dic.Count > max) max = dic.Count;
        }
        return max;
    }
}
```