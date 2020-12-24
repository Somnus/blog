<p>把一个数组最开始的若干个元素搬到数组的末尾，我们称之为数组的旋转。输入一个递增排序的数组的一个旋转，输出旋转数组的最小元素。例如，数组&nbsp;<code>[3,4,5,1,2]</code> 为 <code>[1,2,3,4,5]</code> 的一个旋转，该数组的最小值为1。&nbsp;&nbsp;</p>

<p><strong>示例 1：</strong></p>

<pre><strong>输入：</strong>[3,4,5,1,2]
<strong>输出：</strong>1
</pre>

<p><strong>示例 2：</strong></p>

<pre><strong>输入：</strong>[2,2,2,0,1]
<strong>输出：</strong>0
</pre>

<p>注意：本题与主站 154 题相同：<a href="https://leetcode-cn.com/problems/find-minimum-in-rotated-sorted-array-ii/">https://leetcode-cn.com/problems/find-minimum-in-rotated-sorted-array-ii/</a></p>

### C#代码

```
public class Solution {
    public int MinArray(int[] numbers) {
        int min=numbers[0];
        for(int i=1;i<numbers.Length;i++){
            if(min>numbers[i]){
                min=numbers[i];
            }
        }
        return min;
    }
}
```