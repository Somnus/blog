<p>给定一个非负索引&nbsp;<em>k</em>，其中 <em>k</em>&nbsp;&le;&nbsp;33，返回杨辉三角的第 <em>k </em>行。</p>

<p><img alt="" src="https://upload.wikimedia.org/wikipedia/commons/0/0d/PascalTriangleAnimated2.gif"></p>

<p><small>在杨辉三角中，每个数是它左上方和右上方的数的和。</small></p>

<p><strong>示例:</strong></p>

<pre><strong>输入:</strong> 3
<strong>输出:</strong> [1,3,3,1]
</pre>

<p><strong>进阶：</strong></p>

<p>你可以优化你的算法到 <em>O</em>(<em>k</em>) 空间复杂度吗？</p>

### C#代码

```
public class Solution {
    public IList<int> GetRow(int rowIndex) {
        IList<IList<int>> list = new List<IList<int>>();
        for(int i = 0; i <= rowIndex; i++)
        {
            var midList = new List<int>();
            for(int j = 0; j <= i; j++)
            {
                if(j == 0 || j == i)
                    midList.Add(1);          
                else
                    midList.Add(list[i - 1][j - 1] + list[i - 1][j]);
            }
            list.Add(midList);
        }
        return list[rowIndex];
    }
}
```