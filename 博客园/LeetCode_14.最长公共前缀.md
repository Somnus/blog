<p>编写一个函数来查找字符串数组中的最长公共前缀。</p>

<p>如果不存在公共前缀，返回空字符串&nbsp;<code>&quot;&quot;</code>。</p>

<p><strong>示例&nbsp;1:</strong></p>

<pre><strong>输入: </strong>[&quot;flower&quot;,&quot;flow&quot;,&quot;flight&quot;]
<strong>输出:</strong> &quot;fl&quot;
</pre>

<p><strong>示例&nbsp;2:</strong></p>

<pre><strong>输入: </strong>[&quot;dog&quot;,&quot;racecar&quot;,&quot;car&quot;]
<strong>输出:</strong> &quot;&quot;
<strong>解释:</strong> 输入不存在公共前缀。
</pre>

<p><strong>说明:</strong></p>

<p>所有输入只包含小写字母&nbsp;<code>a-z</code>&nbsp;。</p>

### C#代码

```
public class Solution {
    public string LongestCommonPrefix(string[] strs) {
        /*保证字符串数组为空时不报错*/
        if(strs == null||strs.Length == 0) return "";
        /*如果字符串数组长度为1，则直接返回字符串数组第一个元素。*/
        if(strs.Length == 1) return strs[0];
        
        string str="";
        /*遍历第一个字符串，将strs[0][i]作为基准值，与每个字符串中同顺序字符比较*/
        for(int i = 0;i < strs[0].Length; i++)
        {
            for(int j = 0; j < strs.Length; j++)
            {
                /*如果第一个字符串的遍历次数已经超过某字符串的长度，则直接返回结果*/
                if(i == strs[j].Length) return str;
                /*如果基准值与某字符串同序数字符不相等，则直接返回结果*/
                if(strs[0][i] != strs[j][i]) return str;
                /*如果已经遍历到字符串数组最后一个元素，则将基准值添加到公共前缀中。*/
                if(j == strs.Length - 1) str += strs[0][i];
            }
        }
        return str;
    }
}
```