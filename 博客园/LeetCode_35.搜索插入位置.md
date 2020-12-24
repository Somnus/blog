<p>给定一个排序数组和一个目标值，在数组中找到目标值，并返回其索引。如果目标值不存在于数组中，返回它将会被按顺序插入的位置。</p>

<p>你可以假设数组中无重复元素。</p>

<p><strong>示例 1:</strong></p>

<pre><strong>输入:</strong> [1,3,5,6], 5
<strong>输出:</strong> 2
</pre>

<p><strong>示例&nbsp;2:</strong></p>

<pre><strong>输入:</strong> [1,3,5,6], 2
<strong>输出:</strong> 1
</pre>

<p><strong>示例 3:</strong></p>

<pre><strong>输入:</strong> [1,3,5,6], 7
<strong>输出:</strong> 4
</pre>

<p><strong>示例 4:</strong></p>

<pre><strong>输入:</strong> [1,3,5,6], 0
<strong>输出:</strong> 0
</pre>

### C#代码

```
public class Solution {
    public int SearchInsert(int[] nums, int target) {
        if(nums == null || nums.Length == 0) return 0;
        
        /*比第一个元素还小，插入到最前面，返回0.*/
        if(nums[0] > target) return 0;
        
        /*比最后一个元素还大，插入到最后面，返回数组长度。*/
        if(nums[nums.Length - 1] < target) return nums.Length;
        
        /*使用二分法查找元素*/
        int left = 0;
        int right = nums.Length - 1;
        while(left <= right){
            int mid = (left + right) / 2;
            
            /*找到了目标元素，返回索引值。*/
            if(nums[mid] == target) return mid;
            
            /*左右哨兵指针已经相邻，且目标值处于两值之间，则表示该元素不存在，将插入到右哨兵指针位置。*/
            else if(right - left == 1 && nums[left] < target && nums[right] > target) return right;
            
            /*中间元素比目标元素小，左哨兵指针移动到中间元素位置。*/
            else if(nums[mid] < target) left = mid;
            
            /*中间元素比目标元素大，右哨兵指针移动到中间元素位置。*/
            else right = mid;
        }
        
        /*该处返回任何值都可以，理论上不可能达到此处，只是为了保证函数的正确性。*/
        return -1;
    }
}
```