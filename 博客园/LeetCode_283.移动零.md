<p>给定一个数组 <code>nums</code>，编写一个函数将所有 <code>0</code> 移动到数组的末尾，同时保持非零元素的相对顺序。</p>

<p><strong>示例:</strong></p>

<pre><strong>输入:</strong> <code>[0,1,0,3,12]</code>
<strong>输出:</strong> <code>[1,3,12,0,0]</code></pre>

<p><strong>说明</strong>:</p>

<ol>
	<li>必须在原数组上操作，不能拷贝额外的数组。</li>
	<li>尽量减少操作次数。</li>
</ol>

### C#代码

```
public class Solution {
    public void MoveZeroes(int[] nums) {
        /*
        * 换个角度考虑：只移动非零元素，因为不是零的元素其值就是0，只要非零归位，剩余元素赋值0即可。
        * 从左往右依次遍历，记录0的个数count，索引为i的非零元素向左移到i-count处。
        * 从右往左，对于前count个元素赋值0。
        */
        int count = 0;
        for(int i = 0; i < nums.Length; i++){
            if(nums[i] == 0) count += 1;
            else nums[i - count] = nums[i];
        }
        
        for(int i = 0; i < count; i++){
            nums[nums.Length - 1 - i] = 0;
        }
    }
}
```