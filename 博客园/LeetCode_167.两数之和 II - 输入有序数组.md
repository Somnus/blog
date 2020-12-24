<p>给定一个已按照<strong><em>升序排列</em>&nbsp;</strong>的有序数组，找到两个数使得它们相加之和等于目标数。</p>

<p>函数应该返回这两个下标值<em> </em>index1 和 index2，其中 index1&nbsp;必须小于&nbsp;index2<em>。</em></p>

<p><strong>说明:</strong></p>

<ul>
	<li>返回的下标值（index1 和 index2）不是从零开始的。</li>
	<li>你可以假设每个输入只对应唯一的答案，而且你不可以重复使用相同的元素。</li>
</ul>

<p><strong>示例:</strong></p>

<pre><strong>输入:</strong> numbers = [2, 7, 11, 15], target = 9
<strong>输出:</strong> [1,2]
<strong>解释:</strong> 2 与 7 之和等于目标数 9 。因此 index1 = 1, index2 = 2 。</pre>

### C#代码

```
public class Solution {
    public int[] TwoSum(int[] numbers, int target, int l = 0, int r = int.MaxValue)
    {
        if (r == int.MaxValue) r = numbers.Length - 1;
        int[] array = new int[2];
        int m = l, n = r;
        while (m < n)
        {
            while (numbers[n] > target && m < n) n--;
            if (m < n) array[1] = n;

            while (numbers[m] != target - numbers[array[1]] && m < n) m++;
            if (m < n) array[0] = m;

            if (target == numbers[array[0]] + numbers[array[1]]){
                array[0] += 1;
                array[1] += 1;
                return array;
            }
        }
         return TwoSum(numbers, target, l, n - 1);
    }
}
```