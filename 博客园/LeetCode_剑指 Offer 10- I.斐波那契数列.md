<p>写一个函数，输入 <code>n</code> ，求斐波那契（Fibonacci）数列的第 <code>n</code> 项。斐波那契数列的定义如下：</p>

<pre>
F(0) = 0,   F(1) = 1
F(N) = F(N - 1) + F(N - 2), 其中 N > 1.</pre>

<p>斐波那契数列由 0 和 1 开始，之后的斐波那契数就是由之前的两数相加而得出。</p>

<p>答案需要取模 1e9+7（1000000007），如计算初始结果为：1000000008，请返回 1。</p>

<p> </p>

<p><strong>示例 1：</strong></p>

<pre>
<strong>输入：</strong>n = 2
<strong>输出：</strong>1
</pre>

<p><strong>示例 2：</strong></p>

<pre>
<strong>输入：</strong>n = 5
<strong>输出：</strong>5
</pre>

<p> </p>

<p><strong>提示：</strong></p>

<ul>
	<li><code>0 <= n <= 100</code></li>
</ul>

### C#代码

```
public class Solution {
    Dictionary<int,int> dic = new Dictionary<int,int>();
    public int Fib(int n) {
        if(n == 0) return 0;
        if(n == 1) return 1;
        if(dic.ContainsKey(n)) return dic[n];
        int val = Fib(n - 1) + Fib(n - 2);
        val %= 1000000007;
        dic.Add(n, val);
        return val;
    }
}
```