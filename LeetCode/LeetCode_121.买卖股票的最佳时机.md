<p>给定一个数组，它的第&nbsp;<em>i</em> 个元素是一支给定股票第 <em>i</em> 天的价格。</p>

<p>如果你最多只允许完成一笔交易（即买入和卖出一支股票一次），设计一个算法来计算你所能获取的最大利润。</p>

<p>注意：你不能在买入股票前卖出股票。</p>

<p>&nbsp;</p>

<p><strong>示例 1:</strong></p>

<pre><strong>输入:</strong> [7,1,5,3,6,4]
<strong>输出:</strong> 5
<strong>解释: </strong>在第 2 天（股票价格 = 1）的时候买入，在第 5 天（股票价格 = 6）的时候卖出，最大利润 = 6-1 = 5 。
     注意利润不能是 7-1 = 6, 因为卖出价格需要大于买入价格；同时，你不能在买入前卖出股票。
</pre>

<p><strong>示例 2:</strong></p>

<pre><strong>输入:</strong> [7,6,4,3,1]
<strong>输出:</strong> 0
<strong>解释: </strong>在这种情况下, 没有交易完成, 所以最大利润为 0。
</pre>

### C#代码

```
public class Solution {
    public int MaxProfit(int[] prices) {
        //方法1：时间复杂度O(n^2)，空间复杂度O(1)
        // int result=0;
        // for(int i=0;i<prices.Length;i++)
        // {
        //     for(int j=i+1;j<prices.Length;j++)
        //     {
        //         if(prices[i]<prices[j])
        //         {
        //             int diff=prices[j]-prices[i];
        //             if(diff>result)
        //             {
        //                 result=diff;
        //             }
        //         }
        //     }
        // }
        // return result;

        //方法2：时间复杂度O(n)，空间复杂度O(1)
        int minBuyPrice=int.MaxValue;
        int maxProfit=0;
        for(int i=0;i<prices.Length;i++)
        {
            if(prices[i]<minBuyPrice)
                minBuyPrice=prices[i];

            int diff=prices[i]-minBuyPrice;
            if(maxProfit<diff)
                maxProfit=diff;
        }

        return maxProfit;
    }
}
```