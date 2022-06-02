<p>数组中有一个数字出现的次数超过数组长度的一半，请找出这个数字。</p>

<p>&nbsp;</p>

<p>你可以假设数组是非空的，并且给定的数组总是存在多数元素。</p>

<p>&nbsp;</p>

<p><strong>示例&nbsp;1:</strong></p>

<pre><strong>输入:</strong> [1, 2, 3, 2, 2, 2, 5, 4, 2]
<strong>输出:</strong> 2</pre>

<p>&nbsp;</p>

<p><strong>限制：</strong></p>

<p><code>1 &lt;= 数组长度 &lt;= 50000</code></p>

<p>&nbsp;</p>

<p>注意：本题与主站 169 题相同：<a href="https://leetcode-cn.com/problems/majority-element/">https://leetcode-cn.com/problems/majority-element/</a></p>

<p>&nbsp;</p>

### C#代码

```
public class Solution {
    public int MajorityElement(int[] nums) 
    {
        int count,num;
        count = nums.Length;
        var dic = new Dictionary<int,int>();
        for(int i = 0; i < count; i++)
        {
            int val = nums[i];
            if(dic.TryGetValue(val, out num)) dic.Remove(val);
            num++; 
            if(num > count / 2) return val;
            dic.Add(val, num);
        }

        return count;
    }
}
```