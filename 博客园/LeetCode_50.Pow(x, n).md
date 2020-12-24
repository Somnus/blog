<p>实现&nbsp;<a href="https://www.cplusplus.com/reference/valarray/pow/" target="_blank">pow(<em>x</em>, <em>n</em>)</a>&nbsp;，即计算 x 的 n 次幂函数。</p>

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

<p><strong>说明:</strong></p>

<ul>
	<li>-100.0 &lt;&nbsp;<em>x</em>&nbsp;&lt; 100.0</li>
	<li><em>n</em>&nbsp;是 32 位有符号整数，其数值范围是&nbsp;[&minus;2<sup>31</sup>,&nbsp;2<sup>31&nbsp;</sup>&minus; 1] 。</li>
</ul>

### C#代码

```
public class Solution {
    public double MyPow(double x, int n) {
        /*x=1，或n=0，直接输出1*/
        if(x == 1 || n == 0)
            return 1;

        /*x=-1，n为偶数时输出1，否则-1*/  
        if(x==-1)
            return n%2==0 ? 1 : -1;

        /*n=1，直接输出x*/
        if(n==1)
            return x;

        /*n=-1，输出1/x*/
        if(n==-1)
            return 1/x;

        /*其他情况:x^n= ((x^(n/2))^2) * (x^(n-n/2*2)) */
        double val=MyPow(x,n/2);
        val*=val;
        val*=MyPow(x,n-n/2*2);

        return val;
    }
}
```