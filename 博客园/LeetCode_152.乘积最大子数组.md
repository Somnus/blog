<p>给你一个整数数组 <code>nums</code>&nbsp;，请你找出数组中乘积最大的连续子数组（该子数组中至少包含一个数字），并返回该子数组所对应的乘积。</p>

<p>&nbsp;</p>

<p><strong>示例 1:</strong></p>

<pre><strong>输入:</strong> [2,3,-2,4]
<strong>输出:</strong> <code>6</code>
<strong>解释:</strong>&nbsp;子数组 [2,3] 有最大乘积 6。
</pre>

<p><strong>示例 2:</strong></p>

<pre><strong>输入:</strong> [-2,0,-1]
<strong>输出:</strong> 0
<strong>解释:</strong>&nbsp;结果不能为 2, 因为 [-2,-1] 不是子数组。</pre>

### C#代码

```
public class Solution {
    //乘法遇到负数后，最大值会变成最小值。
    public int MaxProduct(int[] nums) {
        int max = int.MinValue;
        int midMax = 1;
        int midMin = 1;
        int temp;

        for (int i = 0; i < nums.Length; i++)
        {
            if (nums[i] < 0)
            {
                temp = midMax;
                midMax = midMin;
                midMin = temp;
            }

            midMax = Math.Max(midMax * nums[i], nums[i]);
            midMin = Math.Min(midMin * nums[i], nums[i]);
            max = Math.Max(midMax, max);
        }
        return max;
    }
}
```