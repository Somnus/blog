<p>实现函数double Power(double base, int exponent)，求base的exponent次方。不得使用库函数，同时不需要考虑大数问题。</p>

<p>&nbsp;</p>

<p><strong>示例 1:</strong></p>

<pre><strong>输入:</strong> 2.00000, 10
<strong>输出:</strong> 1024.00000
</pre>

<p><strong>示例&nbsp;2:</strong></p>

<pre><strong>输入:</strong> 2.10000, 3
<strong>输出:</strong> 9.26100
</pre>

<p><strong>示例&nbsp;3:</strong></p>

<pre><strong>输入:</strong> 2.00000, -2
<strong>输出:</strong> 0.25000
<strong>解释:</strong> 2<sup>-2</sup> = 1/2<sup>2</sup> = 1/4 = 0.25</pre>

<p>&nbsp;</p>

<p><strong>说明:</strong></p>

<ul>
	<li>-100.0 &lt;&nbsp;<em>x</em>&nbsp;&lt; 100.0</li>
	<li><em>n</em>&nbsp;是 32 位有符号整数，其数值范围是&nbsp;[&minus;2<sup>31</sup>,&nbsp;2<sup>31&nbsp;</sup>&minus; 1] 。</li>
</ul>

<p>注意：本题与主站 50 题相同：<a href="https://leetcode-cn.com/problems/powx-n/">https://leetcode-cn.com/problems/powx-n/</a></p>

### C#代码

```
public class Solution {
        public double MyPow(double x, int n)
        {
            if (x == 0 ||x == 1) return 1;

            if (n == 0) return 1;
            if (n == 1) return x;
            if (n == -1) return 1 / x;
            
            double val = MyPow(x, n / 2);
            val = val * val;
            val = val * MyPow(x, n - n / 2 * 2);
        
            return val;
        }
}
```