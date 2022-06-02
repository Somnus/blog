<p>给定由一些正数（代表长度）组成的数组 <code>A</code>，返回由其中三个长度组成的、<strong>面积不为零</strong>的三角形的最大周长。</p>

<p>如果不能形成任何面积不为零的三角形，返回&nbsp;<code>0</code>。</p>

<p>&nbsp;</p>

<ol>
</ol>

<p><strong>示例 1：</strong></p>

<pre><strong>输入：</strong>[2,1,2]
<strong>输出：</strong>5
</pre>

<p><strong>示例 2：</strong></p>

<pre><strong>输入：</strong>[1,2,1]
<strong>输出：</strong>0
</pre>

<p><strong>示例 3：</strong></p>

<pre><strong>输入：</strong>[3,2,3,4]
<strong>输出：</strong>10
</pre>

<p><strong>示例 4：</strong></p>

<pre><strong>输入：</strong>[3,6,2,3]
<strong>输出：</strong>8
</pre>

<p>&nbsp;</p>

<p><strong>提示：</strong></p>

<ol>
	<li><code>3 &lt;= A.length &lt;= 10000</code></li>
	<li><code>1 &lt;= A[i] &lt;= 10^6</code></li>
</ol>

### C#代码

```
public class Solution {
    public int LargestPerimeter(int[] A) {
        /*如果A为空或者元素个数小于3，无法构成三角形，返回0.*/
        if(A == null || A.Length < 3) return 0;
        
        /*对数组A进行快速排序，左大右小，时间复杂度O(NlogN)。*/
        int[] array = QuickSort(A, 0, A.Length - 1);
        
        /*针对a<b<c，构成三角形的充分必要条件是a<b+c。故基于贪心，遍历判断array中任意相邻三元素是否满足。*/
        for(int i = 0; i < array.Length - 2; i++){
            if(array[i + 1] + array[i + 2] > array[i]) return array[i] + array[i + 1] + array[i + 2];
        }
        
        /*遍历完array后仍未找到满足条件的元素，返回0.*/
        return 0;
    }
    
    private int[] QuickSort(int[] array, int left, int right){
        if(left >= right) return array;
        
        int l = left;
        int r = right;
        int temp = array[l];
        
        while(l < r){
            while(l < r && array[r] <= temp) r--;
            if(l < r) array[l] = array[r];
            while(l < r && array[l] >= temp) l++;
            if(l < r) array[r] =array[l];
        }
        array[l] = temp;
        
        array = QuickSort(array, left, l - 1);
        array = QuickSort(array, l + 1, right);
        return array;
    }
}
```