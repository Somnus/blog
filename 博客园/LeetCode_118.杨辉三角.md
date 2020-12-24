<p>给定一个非负整数&nbsp;<em>numRows，</em>生成杨辉三角的前&nbsp;<em>numRows&nbsp;</em>行。</p>

<p><img alt="" src="https://upload.wikimedia.org/wikipedia/commons/0/0d/PascalTriangleAnimated2.gif"></p>

<p><small>在杨辉三角中，每个数是它左上方和右上方的数的和。</small></p>

<p><strong>示例:</strong></p>

<pre><strong>输入:</strong> 5
<strong>输出:</strong>
[
     [1],
    [1,1],
   [1,2,1],
  [1,3,3,1],
 [1,4,6,4,1]
]</pre>

### C#代码

```
public class Solution {
    public IList<IList<int>> Generate(int numRows) {
        IList<IList<int>> list = new List<IList<int>>();
        for(int i = 0; i < numRows; i++)
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
        return list;
    }
}
```