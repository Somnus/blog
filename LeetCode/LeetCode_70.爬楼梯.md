<p>假设你正在爬楼梯。需要 <em>n</em>&nbsp;阶你才能到达楼顶。</p>

<p>每次你可以爬 1 或 2 个台阶。你有多少种不同的方法可以爬到楼顶呢？</p>

<p><strong>注意：</strong>给定 <em>n</em> 是一个正整数。</p>

<p><strong>示例 1：</strong></p>

<pre><strong>输入：</strong> 2
<strong>输出：</strong> 2
<strong>解释：</strong> 有两种方法可以爬到楼顶。
1.  1 阶 + 1 阶
2.  2 阶</pre>

<p><strong>示例 2：</strong></p>

<pre><strong>输入：</strong> 3
<strong>输出：</strong> 3
<strong>解释：</strong> 有三种方法可以爬到楼顶。
1.  1 阶 + 1 阶 + 1 阶
2.  1 阶 + 2 阶
3.  2 阶 + 1 阶
</pre>

### C#代码

```
public class Solution {

    //递推公式确定：爬上第n阶台阶有两种方法：从第n-1阶爬一个台阶到达；从第n-2阶爬2个台阶到达。则：dp[n]=dp[n-1]+dp[n-2]
    //边界条件：dp[0]=1;dp[1]=1;
    public int ClimbStairs(int n) {
        int[] dp=new int[n+1];
        dp[0]=1;
        dp[1]=1;

        for(int i=2;i<n+1;i++)
            dp[i]=dp[i-1]+dp[i-2];
        
        return dp[n];
    }
}
```