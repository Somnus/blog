<p>给定一个按照升序排列的整数数组 <code>nums</code>，和一个目标值 <code>target</code>。找出给定目标值在数组中的开始位置和结束位置。</p>

<p>如果数组中不存在目标值 <code>target</code>，返回 <code>[-1, -1]</code>。</p>

<p><strong>进阶：</strong></p>

<ul>
	<li>你可以设计并实现时间复杂度为 <code>O(log n)</code> 的算法解决此问题吗？</li>
</ul>

<p> </p>

<p><strong>示例 1：</strong></p>

<pre>
<strong>输入：</strong>nums = [<code>5,7,7,8,8,10]</code>, target = 8
<strong>输出：</strong>[3,4]</pre>

<p><strong>示例 2：</strong></p>

<pre>
<strong>输入：</strong>nums = [<code>5,7,7,8,8,10]</code>, target = 6
<strong>输出：</strong>[-1,-1]</pre>

<p><strong>示例 3：</strong></p>

<pre>
<strong>输入：</strong>nums = [], target = 0
<strong>输出：</strong>[-1,-1]</pre>

<p> </p>

<p><strong>提示：</strong></p>

<ul>
	<li><code>0 <= nums.length <= 10<sup>5</sup></code></li>
	<li><code>-10<sup>9</sup> <= nums[i] <= 10<sup>9</sup></code></li>
	<li><code>nums</code> 是一个非递减数组</li>
	<li><code>-10<sup>9</sup> <= target <= 10<sup>9</sup></code></li>
</ul>

### C#代码

```
public class Solution {
    public int[] SearchRange(int[] nums, int target) {
        int[] result =new int[]{-1, -1};
        if(nums?.Length == 0) return result;
        
        int l = 0;
        int r = nums.Length - 1;
        int mid = 0;
        
        /*查找到target索引mid，不存在直接返回。时间复杂度O(log n)*/
        while(l <= r){
            mid = (l + r) / 2;
            if(nums[mid] == target) break;
            else if(nums[mid] < target) l = mid + 1;
            else r =  mid - 1;
        }   
        if(nums[mid] != target) return result;
        
        /*从mid向前查找最左边target值，一旦小于target立即停止。*/
        for(int i = mid; i > -1; i--){
            if(nums[i] == target) result[0] = i;
            else break;
        }
        
        /*从mid向前查找最左边target值，一旦大于target立即停止。**/
        for(int i = mid; i < nums.Length; i++){
            if(nums[i] == target) result[1] = i;
            else break;
        }
        
        return result;
    }
}
```