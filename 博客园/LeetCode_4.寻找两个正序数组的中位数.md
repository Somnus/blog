<p>给定两个大小为 m 和 n 的正序（从小到大）数组&nbsp;<code>nums1</code> 和&nbsp;<code>nums2</code>。请你找出并返回这两个正序数组的中位数。</p>

<p><strong>进阶：</strong>你能设计一个时间复杂度为 <code>O(log (m+n))</code> 的算法解决此问题吗？</p>

<p>&nbsp;</p>

<p><strong>示例 1：</strong></p>

<pre><strong>输入：</strong>nums1 = [1,3], nums2 = [2]
<strong>输出：</strong>2.00000
<strong>解释：</strong>合并数组 = [1,2,3] ，中位数 2
</pre>

<p><strong>示例 2：</strong></p>

<pre><strong>输入：</strong>nums1 = [1,2], nums2 = [3,4]
<strong>输出：</strong>2.50000
<strong>解释：</strong>合并数组 = [1,2,3,4] ，中位数 (2 + 3) / 2 = 2.5
</pre>

<p><strong>示例 3：</strong></p>

<pre><strong>输入：</strong>nums1 = [0,0], nums2 = [0,0]
<strong>输出：</strong>0.00000
</pre>

<p><strong>示例 4：</strong></p>

<pre><strong>输入：</strong>nums1 = [], nums2 = [1]
<strong>输出：</strong>1.00000
</pre>

<p><strong>示例 5：</strong></p>

<pre><strong>输入：</strong>nums1 = [2], nums2 = []
<strong>输出：</strong>2.00000
</pre>

<p>&nbsp;</p>

<p><strong>提示：</strong></p>

<ul>
	<li><code>nums1.length == m</code></li>
	<li><code>nums2.length == n</code></li>
	<li><code>0 &lt;= m &lt;= 1000</code></li>
	<li><code>0 &lt;= n &lt;= 1000</code></li>
	<li><code>1 &lt;= m + n &lt;= 2000</code></li>
	<li><code>-10<sup>6</sup> &lt;= nums1[i], nums2[i] &lt;= 10<sup>6</sup></code></li>
</ul>

### C#代码

```
public class Solution {
    public double FindMedianSortedArrays(int[] nums1, int[] nums2) {
        /*
        * 中位数两种场景：1）奇数取最中间一个数；2）偶数取最中间两个数均值。
        * 两数组升序排列，最左侧元素最小，依次取出最左侧元素中较小值，按照取出顺序排列，被取出元素数组指针右移。
        * 可以采用新数组储存取出数据，两数组元素全部取出后，新数组中间位置即可求中位数。
        * 时间复杂度O(m+n)，空间复杂度O(m+n)。
        * 可以尝试优化空间复杂度，若对于两数组长度都很大场景，会占用大量的系统内存，甚至造成程序崩溃问题。
        * 优化思路：基于两数组升序排列及中位数特点，当新数组长度达到两数组长度一半时，已经可以计算出中位数了；
        * 无论两数组总长度奇偶性如何，中位数最多与两个数据有关，使用变量记录新数组当前索引指向元素及前序元素即可。
        * 调整后空间复杂度O(1)，时间复杂度O(m+n)【量级未改变，但实际变为原来耗时的一半。】。
        */

        /*中位数索引（针对奇数）或中位数右侧元素索引（针对偶数）。*/
        int m = (nums1.Length + nums2.Length) / 2;

        /*使用临时变量记录上一次被取出元素及当前被取出元素。*/
        int v1 = 0;
        int v2 = 0;

        /*定义访问num1、num2两数组的索引指针*/
        int m1 = 0;
        int m2 = 0;

        /*并未完成对num1、num2任一数组的访问场景。*/
        while(m1 < nums1.Length && m2 < nums2.Length && (m1 + m2) <= m){
            v1 = v2;
            v2 =nums1[m1] <= nums2[m2] ? nums1[m1++] :nums2[m2++];
        } 

        /*针对num1未完成访问场景处理。*/
        while(m1 < nums1.Length && (m1 + m2) <= m){
            v1 = v2;
            v2 = nums1[m1++];
        }

        /*针对num2未完成访问场景处理。*/
        while(m2 < nums2.Length && (m1 + m2) <= m){
            v1 = v2;
            v2 = nums2[m2++];
        }  
        
        /*基于两数组总长度奇偶性返回结果：奇数返回v2，偶数返回v1、v2均值。*/
        return (nums1.Length + nums2.Length) % 2 == 1 ? (double)v2 : (double)(v1 + v2) / 2;
    }
}
```