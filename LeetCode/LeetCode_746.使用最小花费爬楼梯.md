<p>数组的每个索引作为一个阶梯，第&nbsp;<code>i</code>个阶梯对应着一个非负数的体力花费值&nbsp;<code>cost[i]</code>(索引从0开始)。</p>

<p>每当你爬上一个阶梯你都要花费对应的体力花费值，然后你可以选择继续爬一个阶梯或者爬两个阶梯。</p>

<p>您需要找到达到楼层顶部的最低花费。在开始时，你可以选择从索引为 0 或 1 的元素作为初始阶梯。</p>

<p><strong>示例&nbsp;1:</strong></p>

<pre><strong>输入:</strong> cost = [10, 15, 20]
<strong>输出:</strong> 15
<strong>解释:</strong> 最低花费是从cost[1]开始，然后走两步即可到阶梯顶，一共花费15。
</pre>

<p><strong>&nbsp;示例 2:</strong></p>

<pre><strong>输入:</strong> cost = [1, 100, 1, 1, 1, 100, 1, 1, 100, 1]
<strong>输出:</strong> 6
<strong>解释:</strong> 最低花费方式是从cost[0]开始，逐个经过那些1，跳过cost[3]，一共花费6。
</pre>

<p><strong>注意：</strong></p>

<ol>
	<li><code>cost</code>&nbsp;的长度将会在&nbsp;<code>[2, 1000]</code>。</li>
	<li>每一个&nbsp;<code>cost[i]</code> 将会是一个Integer类型，范围为&nbsp;<code>[0, 999]</code>。</li>
</ol>

### C#代码

```
public class Solution {

    public int MinCostClimbingStairs(int[] cost) {
        //从第n-1阶爬1阶达到楼顶，最低花费为：dp[n-1]+cost[n-1];
        //从第n-2阶爬2阶达到楼顶，最低花费为：dp[n-2]+cost[n-2];
        //则状态转移方程为：dp[n]=Math.Min(dp[n-1]+cost[n-1],dp[n-2]+cost[n-2]);
        //边界条件：dp[0]=0;dp[1]=0;
        int length=cost.Length;
        int[] dp=new int[length+1];
        dp[0]=0;
        dp[1]=0;
        for(int i=2;i<length+1;i++){
            dp[i]=Math.Min(dp[i-1]+cost[i-1],dp[i-2]+cost[i-2]);
        }
        return dp[length];
    }
}
```