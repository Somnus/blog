<p>设计一个支持 <code>push</code> ，<code>pop</code> ，<code>top</code> 操作，并能在常数时间内检索到最小元素的栈。</p>

<ul>
	<li><code>push(x)</code> &mdash;&mdash; 将元素 x 推入栈中。</li>
	<li><code>pop()</code>&nbsp;&mdash;&mdash; 删除栈顶的元素。</li>
	<li><code>top()</code>&nbsp;&mdash;&mdash; 获取栈顶元素。</li>
	<li><code>getMin()</code> &mdash;&mdash; 检索栈中的最小元素。</li>
</ul>

<p>&nbsp;</p>

<p><strong>示例:</strong></p>

<pre><strong>输入：</strong>
[&quot;MinStack&quot;,&quot;push&quot;,&quot;push&quot;,&quot;push&quot;,&quot;getMin&quot;,&quot;pop&quot;,&quot;top&quot;,&quot;getMin&quot;]
[[],[-2],[0],[-3],[],[],[],[]]

<strong>输出：</strong>
[null,null,null,null,-3,null,0,-2]

<strong>解释：</strong>
MinStack minStack = new MinStack();
minStack.push(-2);
minStack.push(0);
minStack.push(-3);
minStack.getMin();   --&gt; 返回 -3.
minStack.pop();
minStack.top();      --&gt; 返回 0.
minStack.getMin();   --&gt; 返回 -2.
</pre>

<p>&nbsp;</p>

<p><strong>提示：</strong></p>

<ul>
	<li><code>pop</code>、<code>top</code> 和 <code>getMin</code> 操作总是在 <strong>非空栈</strong> 上调用。</li>
</ul>

### C#代码

```
public class MinStack {

    private Stack<Tuple<int, int>> stack;
    /** initialize your data structure here. */
    public MinStack() {
        stack = new Stack<Tuple<int, int>>();
    }
    
    public void Push(int x) {
        var min=x;
        if(stack.Any()){
            var top = stack.Peek();
            min = Math.Min(top.Item2,min);
        }
        stack.Push(Tuple.Create(x,min));
    }
    
    public void Pop() {
        stack.Pop();
    }
    
    public int Top() {
        var top = stack.Peek();
        return top.Item1;
    }
    
    public int GetMin() {
        var top = stack.Peek();
        return top.Item2;
    }
}

/**
 * Your MinStack object will be instantiated and called as such:
 * MinStack obj = new MinStack();
 * obj.Push(x);
 * obj.Pop();
 * int param_3 = obj.Top();
 * int param_4 = obj.GetMin();
 */
```