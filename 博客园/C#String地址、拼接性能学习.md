<ul>
<li>&nbsp;String类型不可变。定义string变量时会在堆上分配存储空间，而对该变量进行值变更时会重新分配一个存储空间，且保留原存储空间。</li>
</ul>
<p>　　测试思路：获取string类型变量值变更前后的存储空间地址，判断地址是否相同。</p>
<p>　　　　　　获取引用类型地址代码：　　　　　</p>
<div class="cnblogs_code">
<pre>        <span style="color: #0000ff;">public</span> <span style="color: #0000ff;">static</span> <span style="color: #0000ff;">string</span><span style="color: #000000;"> GetMemory(Object o) 
        {
            GCHandle h </span>=<span style="color: #000000;"> GCHandle.Alloc(o, GCHandleType.Pinned);
            IntPtr addr </span>=<span style="color: #000000;"> h.AddrOfPinnedObject();
            </span><span style="color: #0000ff;">return</span> <span style="color: #800000;">"</span><span style="color: #800000;">0x</span><span style="color: #800000;">"</span> + addr.ToString(<span style="color: #800000;">"</span><span style="color: #800000;">X</span><span style="color: #800000;">"</span><span style="color: #000000;">);
        }</span></pre>
</div>
<p>　　　　　　测试代码：</p>
<div class="cnblogs_code">
<pre>            <span style="color: #0000ff;">string</span> str= <span style="color: #800000;">"</span><span style="color: #800000;">hello</span><span style="color: #800000;">"</span><span style="color: #000000;">;
            Console.WriteLine(GetMemory(str));
            str </span>= <span style="color: #800000;">"</span><span style="color: #800000;">hi</span><span style="color: #800000;">"</span><span style="color: #000000;">;
            Console.WriteLine(GetMemory(str));</span></pre>
</div>
<p>　　　　　　测试结果：</p>
<p style="text-align: left;"><img style="display: block; margin-left: auto; margin-right: auto;" src="https://images2018.cnblogs.com/blog/1159701/201805/1159701-20180502113458292-508542839.png" alt="" /></p>
<p>　　　　　测试表明：string类型变量赋值完成后一旦修改值，实际上是重新分配一存储空间存储修改的值，原来的存储空间保留并保存原值。也就证明所谓的&ldquo;string类型值不可变&rdquo;。</p>
<ul>
<li>string字符串拼接性能测试。通过循环模拟实现字符串拼接，并将所运行时间与stringbuilder实现相同功能所需时间对比。　　　　</li>
</ul>
<div class="cnblogs_code">
<pre>  　　　 <span style="color: #0000ff;">public</span> <span style="color: #0000ff;">static</span> <span style="color: #0000ff;">void</span> StringConcat(<span style="color: #0000ff;">int</span><span style="color: #000000;"> num)
        {
            </span><span style="color: #0000ff;">string</span> str = <span style="color: #800000;">""</span><span style="color: #000000;">;
            </span><span style="color: #0000ff;">for</span>(<span style="color: #0000ff;">int</span> i = <span style="color: #800080;">0</span>; i &lt; num; i++<span style="color: #000000;">)
            {
                str </span>+=<span style="color: #000000;"> i.ToString();
            }
        }
        </span><span style="color: #0000ff;">public</span> <span style="color: #0000ff;">static</span> <span style="color: #0000ff;">void</span> StringBuilderTest(<span style="color: #0000ff;">int</span><span style="color: #000000;"> num)
        {
            StringBuilder builder </span>= <span style="color: #0000ff;">new</span><span style="color: #000000;"> StringBuilder();
            </span><span style="color: #0000ff;">for</span>(<span style="color: #0000ff;">int</span> i = <span style="color: #800080;">0</span>; i &lt; num; i++<span style="color: #000000;">)
            {
                builder.Append(i.ToString());
            }
            </span><span style="color: #0000ff;">string</span> str =<span style="color: #000000;"> builder.ToString();
        }</span></pre>
</div>
<p>　　　　测试代码：</p>
<div class="cnblogs_code">
<pre> 　　　　　　 <span style="color: #0000ff;">int</span> num = <span style="color: #800080;">1000</span><span style="color: #000000;">;
            </span><span style="color: #0000ff;">do</span><span style="color: #000000;">
            {
                </span><span style="color: #0000ff;">int</span> start =<span style="color: #000000;"> Environment.TickCount;
                </span><span style="color: #008000;">/*</span><span style="color: #008000;">*****使用字符串连接构建字符串*****</span><span style="color: #008000;">*/</span><span style="color: #000000;">
                StringConcat(num);
                </span><span style="color: #0000ff;">int</span> middle =<span style="color: #000000;"> Environment.TickCount;

                </span><span style="color: #008000;">/*</span><span style="color: #008000;">*****使用StringBuilder构建字符串*****</span><span style="color: #008000;">*/</span><span style="color: #000000;">
                StringBuilderTest(num);
                </span><span style="color: #0000ff;">int</span> end =<span style="color: #000000;"> Environment.TickCount;

                </span><span style="color: #0000ff;">int</span> t1 = middle -<span style="color: #000000;"> start;
                </span><span style="color: #0000ff;">int</span> t2 = end -<span style="color: #000000;"> middle;
                Console.WriteLine(</span><span style="color: #800000;">"</span><span style="color: #800000;">循环次数:{0},StringBuilder:{1}ms,字符串拼接:{2}ms</span><span style="color: #800000;">"</span><span style="color: #000000;">, num, t2, t1);
                num </span>= (<span style="color: #0000ff;">int</span>)(num * <span style="color: #800080;">1.5</span><span style="color: #000000;">);
            } </span><span style="color: #0000ff;">while</span> (num &lt; <span style="color: #800080;">1000000</span>);</pre>
</div>
<p>　　　　测试结果：</p>
<p>　　　　<img src="https://images2018.cnblogs.com/blog/1159701/201805/1159701-20180502124640366-1393501859.png" alt="" /></p>
<p>　　　　结果表明：大量字符串连接性能很差，这当然是由string类型值不可变特性确定的，解决方案是采用stringbuilder代替。</p>
<p>&nbsp;</p>