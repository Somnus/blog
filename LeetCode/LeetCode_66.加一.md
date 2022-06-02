<p>给定一个由 <strong>整数 </strong>组成的<strong> 非空</strong> 数组所表示的非负整数，在该数的基础上加一。</p>

<p>最高位数字存放在数组的首位， 数组中每个元素只存储<strong>单个</strong>数字。</p>

<p>你可以假设除了整数 0 之外，这个整数不会以零开头。</p>

<p> </p>

<p><strong>示例 1：</strong></p>

<pre>
<strong>输入：</strong>digits = [1,2,3]
<strong>输出：</strong>[1,2,4]
<strong>解释：</strong>输入数组表示数字 123。
</pre>

<p><strong>示例 2：</strong></p>

<pre>
<strong>输入：</strong>digits = [4,3,2,1]
<strong>输出：</strong>[4,3,2,2]
<strong>解释：</strong>输入数组表示数字 4321。
</pre>

<p><strong>示例 3：</strong></p>

<pre>
<strong>输入：</strong>digits = [0]
<strong>输出：</strong>[1]
</pre>

<p> </p>

<p><strong>提示：</strong></p>

<ul>
	<li><code>1 <= digits.length <= 100</code></li>
	<li><code>0 <= digits[i] <= 9</code></li>
</ul>

### C#代码

```
public class Solution {
    public int[] PlusOne(int[] digits) {

        bool check = true;
        for(int i = digits.Length-1; i > -1; i--)
        {
            if(check)
            {
                digits[i] += 1;
                if(digits[i] == 10)
                {
                    digits[i] =0;
                    check = true;
                }
                else
                    check = false;
            }
            else
                check = false;
        }

        if(check)
        {
            int[] result = new int[digits.Length + 1];
            result[0] = 1;
            for (int i = 1; i < result.Length; i++)
            {
                result[i] = digits[i - 1];
            }
            return result; 
        }

        return digits;
    }
}
```