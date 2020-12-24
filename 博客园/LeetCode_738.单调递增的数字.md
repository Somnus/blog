<p>给定一个非负整数&nbsp;<code>N</code>，找出小于或等于&nbsp;<code>N</code>&nbsp;的最大的整数，同时这个整数需要满足其各个位数上的数字是单调递增。</p>

<p>（当且仅当每个相邻位数上的数字&nbsp;<code>x</code>&nbsp;和&nbsp;<code>y</code>&nbsp;满足&nbsp;<code>x &lt;= y</code>&nbsp;时，我们称这个整数是单调递增的。）</p>

<p><strong>示例 1:</strong></p>

<pre><strong>输入:</strong> N = 10
<strong>输出:</strong> 9
</pre>

<p><strong>示例 2:</strong></p>

<pre><strong>输入:</strong> N = 1234
<strong>输出:</strong> 1234
</pre>

<p><strong>示例 3:</strong></p>

<pre><strong>输入:</strong> N = 332
<strong>输出:</strong> 299
</pre>

<p><strong>说明:</strong> <code>N</code>&nbsp;是在&nbsp;<code>[0, 10^9]</code>&nbsp;范围内的一个整数。</p>

### C#代码

```
public class Solution {
    public int MonotoneIncreasingDigits(int N) { 
        /*
        * 将N转换为数组array，数组长度n。
        * 假设N满足递增，则从右往左遍历array检查假设是否成立：对于任意下标i，array[i] >= array[i - 1]。
        * 如果某下标i不满足递增条件，则做如下处理：array[i]置9，array[i-1]减1。
        * 做此处理原因：假设可得0 ~ i-1递增，i ~ i-1 递减，则需要调整i及i-1处值以保证递增性。
        * 递增性调整说明：i-1处只能做两种调整：不变或减小；
        *   方案1）i-1处不变，i处增大，满足"单调递增"，不满足"目标值不大于N"。
        *   方案2）i-1处不变，i处减小，不满足"单调递增"，满足"目标值不大于N"。
        *   方案3）i-1处减小，i处不变，可能满足"单调递增"，满足"目标值不大于N"。
        *   方案4）i-1处减小，i处增大，可能满足"单调递增"，满足"目标值不大于N"。
        * 方案3、4除了不一定满足"单调递增"外，还可能存在漏值情况，即找到的目标是不是满足条件的最大值。
        * 改良方案4，将i处增大值9，可以保证单调性，且在i-1减小情况下保证取到目标值最大。
        */
        
        /*N转为char[]，未转换为string的原因：string不可变性决定不能修改值。*/
        char[] array = N.ToString().ToCharArray();
        
        /*记录从左往右不满足单调递增的起始元素下标*/
        int flag = array.Length;
        
        /*从右往左遍历数组，一旦出现左侧元素大于当前元素，将左侧元素减1，并更新flag值为当前元素下标。*/
        for(int i = array.Length - 1; i > 0; i--){
            if(array[i - 1] <= array[i]) continue;
            array[i - 1] = (char)(array[i - 1] - 1);
            flag = i;    
        }
        
        for(int i = flag; i < array.Length; i++){
            array[i] = '9';
        }
        
        return Convert.ToInt32(new string(array));
    }
}
```