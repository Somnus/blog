<p>给定一个整数数组，找出总和最大的连续数列，并返回总和。</p>

<p><strong>示例：</strong></p>

<pre><strong>输入：</strong> [-2,1,-3,4,-1,2,1,-5,4]
<strong>输出：</strong> 6
<strong>解释：</strong> 连续子数组 [4,-1,2,1] 的和最大，为 6。
</pre>

<p><strong>进阶：</strong></p>

<p>如果你已经实现复杂度为 O(<em>n</em>) 的解法，尝试使用更为精妙的分治法求解。</p>

### C#代码

```
public class Solution {
    public int MaxSubArray(int[] nums)
    {
        if (nums.Length == 0) return 0;

        int max = nums[0];
        int currentSum = nums[0];
        int temp;
        for (int i = 1; i < nums.Length; i++)
        {
            temp = currentSum + nums[i];
            if (temp >= nums[i]) currentSum = temp;
            else currentSum = nums[i];
            if (max < currentSum) max = currentSum;
        }
        return max;
    }
}
```