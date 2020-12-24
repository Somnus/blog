<p>给定一个无序的数组，找出数组在排序之后，相邻元素之间最大的差值。</p>

<p>如果数组元素个数小于 2，则返回 0。</p>

<p><strong>示例&nbsp;1:</strong></p>

<pre><strong>输入:</strong> [3,6,9,1]
<strong>输出:</strong> 3
<strong>解释:</strong> 排序后的数组是 [1,3,6,9]<strong><em>, </em></strong>其中相邻元素 (3,6) 和 (6,9) 之间都存在最大差值 3。</pre>

<p><strong>示例&nbsp;2:</strong></p>

<pre><strong>输入:</strong> [10]
<strong>输出:</strong> 0
<strong>解释:</strong> 数组元素个数小于 2，因此返回 0。</pre>

<p><strong>说明:</strong></p>

<ul>
	<li>你可以假设数组中所有元素都是非负整数，且数值在 32 位有符号整数范围内。</li>
	<li>请尝试在线性时间复杂度和空间复杂度的条件下解决此问题。</li>
</ul>

### C#代码

```
public class Solution {
    public int MaximumGap(int[] nums) {
        if(nums?.Length == 0) return 0;
        
        /*快速排序，时间复杂度O(n log n)*/
        nums = QuickSort(nums, 0, nums.Length - 1);
        
        /*遍历获取相邻元素的最大差值，时间复杂度O(n)*/
        int max = 0;
        for(int i = 1; i < nums.Length; i++){
            int val = nums[i] - nums[i - 1];
            if(val > max) max = val;
        }
        
        return max;
    }
    
    public int[] QuickSort(int[] nums, int left, int right){
        if(left >= right) return nums;
        
        int l = left;
        int r = right;
        int temp = nums[left];
        while(l < r){
            while(l < r && temp <= nums[r]) r--;
            if(l < r) nums[l] = nums[r];
            while(l < r && temp >= nums[l]) l++;
            if(l < r) nums[r] = nums[l];
        }
        nums[l] = temp;
        
        nums = QuickSort(nums, left, l - 1);
        nums = QuickSort(nums, l + 1, right);
        
        return nums;
    }
}
```