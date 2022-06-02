<p>输入数字 <code>n</code>，按顺序打印出从 1 到最大的 n 位十进制数。比如输入 3，则打印出 1、2、3 一直到最大的 3 位数 999。</p>

<p><strong>示例 1:</strong></p>

<pre><strong>输入:</strong> n = 1
<strong>输出:</strong> [1,2,3,4,5,6,7,8,9]
</pre>

<p>&nbsp;</p>

<p>说明：</p>

<ul>
	<li>用返回一个整数列表来代替打印</li>
	<li>n 为正整数</li>
</ul>

### C#代码

```
public class Solution {
    /*虽然通过了，但是题目并未限制n的大小，后续需要思考当数组长度已经超出int最大值场景。*/
    public int[] PrintNumbers(int n) {
        int num=1;
        for(int i=1;i<n+1;i++){
            num*=10;
        }
        int[] nums=new int[num-1];
        for(int i=0;i<nums.Length;i++){
            nums[i]=i+1;
        }
        return nums;
    }
}
```