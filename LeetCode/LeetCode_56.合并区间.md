<p>给出一个区间的集合，请合并所有重叠的区间。</p>

<p>&nbsp;</p>

<p><strong>示例 1:</strong></p>

<pre><strong>输入:</strong> intervals = [[1,3],[2,6],[8,10],[15,18]]
<strong>输出:</strong> [[1,6],[8,10],[15,18]]
<strong>解释:</strong> 区间 [1,3] 和 [2,6] 重叠, 将它们合并为 [1,6].
</pre>

<p><strong>示例&nbsp;2:</strong></p>

<pre><strong>输入:</strong> intervals = [[1,4],[4,5]]
<strong>输出:</strong> [[1,5]]
<strong>解释:</strong> 区间 [1,4] 和 [4,5] 可被视为重叠区间。</pre>

<p><strong>注意：</strong>输入类型已于2019年4月15日更改。 请重置默认代码定义以获取新方法签名。</p>

<p>&nbsp;</p>

<p><strong>提示：</strong></p>

<ul>
	<li><code>intervals[i][0] &lt;= intervals[i][1]</code></li>
</ul>

### C#代码

```
public class Solution {
    public int[][] Merge(int[][] intervals)
    {
        if (intervals.Length == 0)
        {
            return intervals;
        }

        /*基于每个区间左边界完成数组排序，保证区间左边界越小的越靠近左边。*/
        intervals = intervals.OrderBy(p => p[0]).ToArray();

        /*遍历数组，比较相邻区间是否能合并。如果左区间的右边界不小于右区间的左边界，则左右区间可以合并。*/
        List<int[]> list = new List<int[]>();
        for (int i = 0; i < intervals.Length - 1; i++)
        {
            /*
            左区间的右边界不小于右区间的左边界，则区间可以合并。将右区间作为合并后结果，
            则更新右区间的左边界为左区间的左边界。
            */
            if (intervals[i][1] >= intervals[i + 1][0])
            {
                intervals[i + 1][0] = intervals[i][0];

                /*左区间的右边界不小于右区间的右边界，则右区间的右边界更新为左区间的右边界。*/
                if (intervals[i][1] >= intervals[i + 1][1])
                {
                    intervals[i + 1][1] = intervals[i][1];
                }
            }

            /*左区间的右边界小于右区间的左边界，则左区间不能与右区间合并，将左区间添加到结果数组中。*/
            else
            {
                list.Add(intervals[i]);
            }
        }
        /*将数组中最后一个元素添加到结果中。*/
        list.Add(intervals[intervals.Length - 1]);

        int[][] result = list.ToArray();
        return result;
    }
}
```