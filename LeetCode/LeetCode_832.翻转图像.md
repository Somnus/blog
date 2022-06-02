<p>给定一个二进制矩阵&nbsp;<code>A</code>，我们想先水平翻转图像，然后反转图像并返回结果。</p>

<p>水平翻转图片就是将图片的每一行都进行翻转，即逆序。例如，水平翻转&nbsp;<code>[1, 1, 0]</code>&nbsp;的结果是&nbsp;<code>[0, 1, 1]</code>。</p>

<p>反转图片的意思是图片中的&nbsp;<code>0</code>&nbsp;全部被&nbsp;<code>1</code>&nbsp;替换，&nbsp;<code>1</code>&nbsp;全部被&nbsp;<code>0</code>&nbsp;替换。例如，反转&nbsp;<code>[0, 1, 1]</code>&nbsp;的结果是&nbsp;<code>[1, 0, 0]</code>。</p>

<p><strong>示例 1:</strong></p>

<pre>
<strong>输入: </strong>[[1,1,0],[1,0,1],[0,0,0]]
<strong>输出: </strong>[[1,0,0],[0,1,0],[1,1,1]]
<strong>解释:</strong> 首先翻转每一行: [[0,1,1],[1,0,1],[0,0,0]]；
     然后反转图片: [[1,0,0],[0,1,0],[1,1,1]]
</pre>

<p><strong>示例 2:</strong></p>

<pre>
<strong>输入: </strong>[[1,1,0,0],[1,0,0,1],[0,1,1,1],[1,0,1,0]]
<strong>输出: </strong>[[1,1,0,0],[0,1,1,0],[0,0,0,1],[1,0,1,0]]
<strong>解释:</strong> 首先翻转每一行: [[0,0,1,1],[1,0,0,1],[1,1,1,0],[0,1,0,1]]；
     然后反转图片: [[1,1,0,0],[0,1,1,0],[0,0,0,1],[1,0,1,0]]
</pre>

<p><strong>说明:</strong></p>

<ul>
	<li><code>1 &lt;= A.length = A[0].length &lt;= 20</code></li>
	<li><code>0 &lt;= A[i][j]&nbsp;&lt;=&nbsp;1</code></li>
</ul>

### C#代码

```
public class Solution {
    public int[][] FlipAndInvertImage(int[][] A) {
        for(int i=0;i<A.Length;i++){
            int length=A[i].Length;
            for(int j=0;j<length/2;j++){
                int temp=A[i][j];
                A[i][j]=A[i][length-j-1];
                A[i][length-j-1]=temp;
            }
            for(int j=0;j<length;j++){
                A[i][j]=A[i][j]==1?0:1;
            }
        }
        return A;
    }
}
```