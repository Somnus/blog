<p>给定一组非负整数 <code>nums</code>，重新排列它们每个数字的顺序（每个数字不可拆分）使之组成一个最大的整数。</p>

<p><strong>注意：</strong>输出结果可能非常大，所以你需要返回一个字符串而不是整数。</p>

<p> </p>

<p><strong>示例 1：</strong></p>

<pre>
<strong>输入<code>：</code></strong><code>nums = [10,2]</code>
<strong>输出：</strong><code>"210"</code></pre>

<p><strong>示例 2：</strong></p>

<pre>
<strong>输入<code>：</code></strong><code>nums = [3,30,34,5,9]</code>
<strong>输出：</strong><code>"9534330"</code>
</pre>

<p><strong>示例 3：</strong></p>

<pre>
<strong>输入<code>：</code></strong>nums = [1]
<strong>输出：</strong>"1"
</pre>

<p><strong>示例 4：</strong></p>

<pre>
<strong>输入<code>：</code></strong>nums = [10]
<strong>输出：</strong>"10"
</pre>

<p> </p>

<p><strong>提示：</strong></p>

<ul>
	<li><code>1 <= nums.length <= 100</code></li>
	<li><code>0 <= nums[i] <= 10<sup>9</sup></code></li>
</ul>

### C#代码

```
public class Solution {
    public string LargestNumber(int[] nums){
        StringBuilder sb = new StringBuilder();
        nums = QuickSort(nums);
        
        if(nums[nums.Length-1]==0){
            return "0";    
        }
    
        for (int i = nums.Length - 1; i >= 0; i--){
            sb.Append(nums[i]);
        }

        string result = sb.ToString();
        return result;
    }

        public int[] QuickSort(int[] array, int left = int.MinValue, int right = int.MaxValue)
        {
            if (left == int.MinValue){
                left = 0;
            }
            if (right == int.MaxValue){
                right = array.Length - 1;
            }

            if (left > right){
                return array;
            }

            int i = left;
            int j = right;
            int temp = array[left];

            while (i < j){
                while (DiffNumber(array[j], temp) && i < j){
                    j--;
                }
                if (i < j){
                    array[i] = array[j];
                    i++;
                }

                while (DiffNumber(temp, array[i]) && i < j){
                    i++;
                }

                if (i < j){
                    array[j] = array[i];
                    j--;
                }
            }
            
            array[i] = temp;

            array = QuickSort(array, left, i - 1);
            array = QuickSort(array, i + 1, right);

            return array;
        }
        public bool DiffNumber(int num1, int num2){
            string val1 = num1.ToString() + num2.ToString();
            string val2 = num2.ToString() + num1.ToString();

            for (int i = 0; i < val1.Length; i++){
                if (val1[i] > val2[i]){
                    return true;
                }

                if (val1[i] < val2[i]){
                    return false;
                }
            }
            return true;
        }
}
```