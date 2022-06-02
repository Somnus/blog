<p>给定一个大小为 <em>n </em>的数组，找到其中的多数元素。多数元素是指在数组中出现次数<strong>大于</strong>&nbsp;<code>&lfloor; n/2 &rfloor;</code>&nbsp;的元素。</p>

<p>你可以假设数组是非空的，并且给定的数组总是存在多数元素。</p>

<p>&nbsp;</p>

<p><strong>示例&nbsp;1:</strong></p>

<pre><strong>输入:</strong> [3,2,3]
<strong>输出:</strong> 3</pre>

<p><strong>示例&nbsp;2:</strong></p>

<pre><strong>输入:</strong> [2,2,1,1,1,2,2]
<strong>输出:</strong> 2
</pre>

### C#代码

```
public class Solution {
    public int MajorityElement(int[] nums) {
        Dictionary<int,int> dic = new Dictionary<int,int>();
        for(int i = 0; i < nums.Length; i++){
            if(dic.ContainsKey(nums[i]))dic[nums[i]] += 1;
            else dic.Add(nums[i], 1);    
            if(dic[nums[i]] > nums.Length / 2) return nums[i];
        }
        return 0;
    }
}
```