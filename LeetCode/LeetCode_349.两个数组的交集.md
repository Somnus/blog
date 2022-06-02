<p>给定两个数组，编写一个函数来计算它们的交集。</p>

<p>&nbsp;</p>

<p><strong>示例 1：</strong></p>

<pre><strong>输入：</strong>nums1 = [1,2,2,1], nums2 = [2,2]
<strong>输出：</strong>[2]
</pre>

<p><strong>示例 2：</strong></p>

<pre><strong>输入：</strong>nums1 = [4,9,5], nums2 = [9,4,9,8,4]
<strong>输出：</strong>[9,4]</pre>

<p>&nbsp;</p>

<p><strong>说明：</strong></p>

<ul>
	<li>输出结果中的每个元素一定是唯一的。</li>
	<li>我们可以不考虑输出结果的顺序。</li>
</ul>

### C#代码

```
public class Solution {
    public int[] Intersection(int[] nums1, int[] nums2) {
        Dictionary<int, int> dic = new Dictionary<int, int>();
        foreach (var item in nums1)
        {
            if (dic.ContainsKey(item) == false)
            {
                dic.Add(item, item);
            }
        }

        List<int> list = new List<int>();
        foreach (var item in nums2)
        {
            if (dic.ContainsKey(item))
            {
                list.Add(item);
                dic.Remove(item);
            }
        }

        int[] array = list.ToArray();
        return array;
    }
}
```