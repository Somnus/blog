<p>给定一个非负整数数组，你最初位于数组的第一个位置。</p>

<p>数组中的每个元素代表你在该位置可以跳跃的最大长度。</p>

<p>判断你是否能够到达最后一个位置。</p>

<p><strong>示例&nbsp;1:</strong></p>

<pre><strong>输入:</strong> [2,3,1,1,4]
<strong>输出:</strong> true
<strong>解释:</strong> 我们可以先跳 1 步，从位置 0 到达 位置 1, 然后再从位置 1 跳 3 步到达最后一个位置。
</pre>

<p><strong>示例&nbsp;2:</strong></p>

<pre><strong>输入:</strong> [3,2,1,0,4]
<strong>输出:</strong> false
<strong>解释:</strong> 无论怎样，你总会到达索引为 3 的位置。但该位置的最大跳跃长度是 0 ， 所以你永远不可能到达最后一个位置。
</pre>

### C#代码

```
public class Solution {
    public bool CanJump(int[] nums)
    {
        /*抄官方，核心：可以到达位置i，一定可到达[i,x+nums[i]]区间任意位置。*/
        int max = 0;
        for( int i = 0; i < nums.Length; i++){
            if(max < i ){
                return false;
            }
            max = Math.Max(i + nums[i],max);
        }
        return true;
    }
}
```