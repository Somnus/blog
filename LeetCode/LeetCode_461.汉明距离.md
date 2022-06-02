<p>两个整数之间的<a href="https://baike.baidu.com/item/%E6%B1%89%E6%98%8E%E8%B7%9D%E7%A6%BB">汉明距离</a>指的是这两个数字对应二进制位不同的位置的数目。</p>

<p>给出两个整数 <code>x</code> 和 <code>y</code>，计算它们之间的汉明距离。</p>

<p><strong>注意：</strong><br />
0 &le; <code>x</code>, <code>y</code> &lt; 2<sup>31</sup>.</p>

<p><strong>示例:</strong></p>

<pre>
<strong>输入:</strong> x = 1, y = 4

<strong>输出:</strong> 2

<strong>解释:</strong>
1   (0 0 0 1)
4   (0 1 0 0)
       &uarr;   &uarr;

上面的箭头指出了对应二进制位不同的位置。
</pre>

### C#代码

```
public class Solution {
    public int HammingDistance(int x, int y) {
        var byte1 = Convert.ToString(x, 2);
        var byte2 = Convert.ToString(y, 2);

        int diffLength = byte1.Length - byte2.Length;
        string str = "";
        for (int i = 0; i < Math.Abs(diffLength); i++)
            str += '0';

        if (diffLength > 0)
            byte2 = str + byte2;

        if (diffLength < 0)
            byte1 = str + byte1;

        int num = 0;
        for (int i = 0; i < byte1.Length; i++)
        {
            if (byte1[i] != byte2[i])
                num += 1;
        }

        return num;
    }
}
```