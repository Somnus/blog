<p>给定一个二进制数组， 计算其中最大连续1的个数。</p>

<p><strong>示例 1:</strong></p>

<pre>
<strong>输入:</strong> [1,1,0,1,1,1]
<strong>输出:</strong> 3
<strong>解释:</strong> 开头的两位和最后的三位都是连续1，所以最大连续1的个数是 3.
</pre>

<p><strong>注意：</strong></p>

<ul>
	<li>输入的数组只包含&nbsp;<code>0</code> 和<code>1</code>。</li>
	<li>输入数组的长度是正整数，且不超过 10,000。</li>
</ul>

### C#代码

```
public class Solution {
    public int FindMaxConsecutiveOnes(int[] nums) {
        int num = 0;
        int index = 0;
        int length = nums.Length;

        int[] array = new int[length + 2];
        array[0] = 0;
        array[length + 1] = 0;

        for(int i = 0; i < nums.Length; i++)
        {
            array[i + 1] = nums[i];
        }

        for(int i = 0; i < length + 2; i++)
        {
            if(array[i] == 0)
            {
                int diff = i - index - 1;
                if(diff > num)
                {
                    num = diff;
                }
                index = i;
            }
        }
        return num;
    }
}
```