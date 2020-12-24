<p><strong>MVC控制器向视图传递数据包含多个实体类的解决方案有很多，这里主要针对视图模型、动态模型以及Tuple三种方法进行一些总结与记录。</strong></p>
<p><strong>基础集合类：TableA</strong></p>
<div class="cnblogs_code">
<pre><span style="color: #0000ff;">namespace</span><span style="color: #000000;"> ViewModelStudy.Models
{
    </span><span style="color: #0000ff;">public</span> <span style="color: #0000ff;">class</span><span style="color: #000000;"><strong> TableA</strong>
    {
        </span><span style="color: #0000ff;">public</span> <span style="color: #0000ff;">int</span> A { <span style="color: #0000ff;">get</span>; <span style="color: #0000ff;">set</span><span style="color: #000000;">; }
        </span><span style="color: #0000ff;">public</span> <span style="color: #0000ff;">int</span> B { <span style="color: #0000ff;">get</span>; <span style="color: #0000ff;">set</span><span style="color: #000000;">; }
        </span><span style="color: #0000ff;">public</span> <span style="color: #0000ff;">int</span> C { <span style="color: #0000ff;">get</span>; <span style="color: #0000ff;">set</span><span style="color: #000000;">; }
    }
}</span></pre>
</div>
<p><strong>基础集合类：TableB</strong></p>
<div class="cnblogs_code">
<pre><span style="color: #0000ff;">namespace</span><span style="color: #000000;"> ViewModelStudy.Models
{
    </span><span style="color: #0000ff;">public</span> <span style="color: #0000ff;">class</span><span style="color: #000000;"><strong> TableB</strong>
    {
        </span><span style="color: #0000ff;">public</span> <span style="color: #0000ff;">int</span> X { <span style="color: #0000ff;">get</span>; <span style="color: #0000ff;">set</span><span style="color: #000000;">; }
        </span><span style="color: #0000ff;">public</span> <span style="color: #0000ff;">int</span> Y { <span style="color: #0000ff;">get</span>; <span style="color: #0000ff;">set</span><span style="color: #000000;">; }
        </span><span style="color: #0000ff;">public</span> <span style="color: #0000ff;">int</span> Z { <span style="color: #0000ff;">get</span>; <span style="color: #0000ff;">set</span><span style="color: #000000;">; }
    }
}</span></pre>
</div>
<p><strong>构建分别以TableA，TableB为基础的集合</strong></p>
<div class="cnblogs_code">
<pre> <span style="color: #0000ff;">public</span> List&lt;TableA&gt;<span style="color: #000000;"><strong> tableA</strong>()
        {
            </span><span style="color: #0000ff;">var</span> table = <span style="color: #0000ff;">new</span> List&lt;TableA&gt;<span style="color: #000000;">()
            {
                </span><span style="color: #0000ff;">new</span> TableA{A=<span style="color: #800080;">1</span>,B=<span style="color: #800080;">2</span>,C=<span style="color: #800080;">3</span><span style="color: #000000;">},
                </span><span style="color: #0000ff;">new</span> TableA{A=<span style="color: #800080;">4</span>,B=<span style="color: #800080;">5</span>,C=<span style="color: #800080;">6</span><span style="color: #000000;">}
            };
            </span><span style="color: #0000ff;">return</span><span style="color: #000000;"> table;
        }
 </span><span style="color: #0000ff;">public</span> List&lt;TableB&gt;<span style="color: #000000;"><strong> tableB</strong>()
        {
            </span><span style="color: #0000ff;">var</span> table = <span style="color: #0000ff;">new</span> List&lt;TableB&gt;<span style="color: #000000;">()
            {
                </span><span style="color: #0000ff;">new</span> TableB{X=<span style="color: #800080;">1</span>,Y=<span style="color: #800080;">2</span>,Z=<span style="color: #800080;">3</span><span style="color: #000000;">},
                </span><span style="color: #0000ff;">new</span> TableB{X=<span style="color: #800080;">4</span>,Y=<span style="color: #800080;">5</span>,Z=<span style="color: #800080;">6</span><span style="color: #000000;">}
            };
            </span><span style="color: #0000ff;">return</span><span style="color: #000000;"> table;
        }</span></pre>
</div>
<p><strong>方法一：新建ViewModel向视图传递集合数据</strong></p>
<div class="cnblogs_code">
<pre><span style="color: #0000ff;">using</span><span style="color: #000000;"> System.Collections.Generic;
</span><span style="color: #0000ff;">namespace</span><span style="color: #000000;"> ViewModelStudy.Models
{
    </span><span style="color: #0000ff;">public</span> <span style="color: #0000ff;">class</span><span style="color: #000000;"><strong><span style="background-color: #ffffff;"> ViewTable</span></strong>
    {
        </span><span style="color: #0000ff;">public</span> List&lt;TableA&gt; TableA { <span style="color: #0000ff;">get</span>; <span style="color: #0000ff;">set</span><span style="color: #000000;">; }
        </span><span style="color: #0000ff;">public</span> List&lt;TableB&gt; TableB { <span style="color: #0000ff;">get</span>; <span style="color: #0000ff;">set</span><span style="color: #000000;">; }
    }
}<br /></span></pre>
</div>
<div class="cnblogs_code">
<pre> <span style="color: #0000ff;">public</span><span style="color: #000000;"> ActionResult ViewModel()
        {
            </span><span style="color: #0000ff;">var</span> ViewTable = <span style="color: #0000ff;">new</span><span style="color: #000000;"> ViewTable()
            {
                TableA </span>=<span style="color: #000000;"> tableA(),
                TableB </span>=<span style="color: #000000;"> tableB()
            };
            </span><span style="color: #0000ff;">return</span><span style="color: #000000;"> View(ViewTable);
        }</span></pre>
</div>
<div class="cnblogs_code">
<pre><span style="color: #000000;"><span style="background-color: #ffff00;">@using ViewModelStudy.Models
@model ViewTable</span>
@{
    Layout = null;
}

</span><span style="color: #0000ff;">&lt;!</span><span style="color: #ff00ff;">DOCTYPE html</span><span style="color: #0000ff;">&gt;</span>

<span style="color: #0000ff;">&lt;</span><span style="color: #800000;">html</span><span style="color: #0000ff;">&gt;</span>
<span style="color: #0000ff;">&lt;</span><span style="color: #800000;">head</span><span style="color: #0000ff;">&gt;</span>
    <span style="color: #0000ff;">&lt;</span><span style="color: #800000;">title</span><span style="color: #0000ff;">&gt;</span>Index<span style="color: #0000ff;">&lt;/</span><span style="color: #800000;">title</span><span style="color: #0000ff;">&gt;</span>
<span style="color: #0000ff;">&lt;/</span><span style="color: #800000;">head</span><span style="color: #0000ff;">&gt;</span>
<span style="color: #0000ff;">&lt;</span><span style="color: #800000;">body</span><span style="color: #0000ff;">&gt;</span>
    <span style="color: #0000ff;">&lt;</span><span style="color: #800000;">div</span><span style="color: #0000ff;">&gt;</span>
    <span style="color: #0000ff;">&lt;</span><span style="color: #800000;">table </span><span style="color: #ff0000;">class</span><span style="color: #0000ff;">="table1"</span><span style="color: #0000ff;">&gt;</span>
        <span style="color: #0000ff;">&lt;</span><span style="color: #800000;">tbody</span><span style="color: #0000ff;">&gt;</span><span style="color: #000000;">
        @foreach (var item in Model.TableA)
        {
            </span><span style="color: #0000ff;">&lt;</span><span style="color: #800000;">tr</span><span style="color: #0000ff;">&gt;</span>
                <span style="color: #0000ff;">&lt;</span><span style="color: #800000;">td</span><span style="color: #0000ff;">&gt;</span>@item.A<span style="color: #0000ff;">&lt;/</span><span style="color: #800000;">td</span><span style="color: #0000ff;">&gt;</span>
                <span style="color: #0000ff;">&lt;</span><span style="color: #800000;">td</span><span style="color: #0000ff;">&gt;</span>@item.B<span style="color: #0000ff;">&lt;/</span><span style="color: #800000;">td</span><span style="color: #0000ff;">&gt;</span>
                <span style="color: #0000ff;">&lt;</span><span style="color: #800000;">td</span><span style="color: #0000ff;">&gt;</span>@item.C<span style="color: #0000ff;">&lt;/</span><span style="color: #800000;">td</span><span style="color: #0000ff;">&gt;</span>
            <span style="color: #0000ff;">&lt;/</span><span style="color: #800000;">tr</span><span style="color: #0000ff;">&gt;</span><span style="color: #000000;">
        }
        </span><span style="color: #0000ff;">&lt;/</span><span style="color: #800000;">tbody</span><span style="color: #0000ff;">&gt;</span>
    <span style="color: #0000ff;">&lt;/</span><span style="color: #800000;">table</span><span style="color: #0000ff;">&gt;</span>
    <span style="color: #0000ff;">&lt;</span><span style="color: #800000;">table </span><span style="color: #ff0000;">class</span><span style="color: #0000ff;">="table2"</span><span style="color: #0000ff;">&gt;</span>
        <span style="color: #0000ff;">&lt;</span><span style="color: #800000;">tbody</span><span style="color: #0000ff;">&gt;</span><span style="color: #000000;">
        @foreach (var item in Model.TableB)
        {
            </span><span style="color: #0000ff;">&lt;</span><span style="color: #800000;">tr</span><span style="color: #0000ff;">&gt;</span>
                <span style="color: #0000ff;">&lt;</span><span style="color: #800000;">td</span><span style="color: #0000ff;">&gt;</span>@item.X<span style="color: #0000ff;">&lt;/</span><span style="color: #800000;">td</span><span style="color: #0000ff;">&gt;</span>
                <span style="color: #0000ff;">&lt;</span><span style="color: #800000;">td</span><span style="color: #0000ff;">&gt;</span>@item.Y<span style="color: #0000ff;">&lt;/</span><span style="color: #800000;">td</span><span style="color: #0000ff;">&gt;</span>
                <span style="color: #0000ff;">&lt;</span><span style="color: #800000;">td</span><span style="color: #0000ff;">&gt;</span>@item.Z<span style="color: #0000ff;">&lt;/</span><span style="color: #800000;">td</span><span style="color: #0000ff;">&gt;</span>
            <span style="color: #0000ff;">&lt;/</span><span style="color: #800000;">tr</span><span style="color: #0000ff;">&gt;</span><span style="color: #000000;">
        }
        </span><span style="color: #0000ff;">&lt;/</span><span style="color: #800000;">tbody</span><span style="color: #0000ff;">&gt;</span>
    <span style="color: #0000ff;">&lt;/</span><span style="color: #800000;">table</span><span style="color: #0000ff;">&gt;</span>
    <span style="color: #0000ff;">&lt;/</span><span style="color: #800000;">div</span><span style="color: #0000ff;">&gt;</span>
<span style="color: #0000ff;">&lt;/</span><span style="color: #800000;">body</span><span style="color: #0000ff;">&gt;</span>
<span style="color: #0000ff;">&lt;/</span><span style="color: #800000;">html</span><span style="color: #0000ff;">&gt;</span></pre>
</div>
<p><strong>方法二：使用dynamic传递数据</strong></p>
<div class="cnblogs_code">
<pre>        <span style="color: #0000ff;">public</span><span style="color: #000000;"> ActionResult ExpandoObject()
        {
            </span><span style="color: #0000ff;">dynamic</span> table = <span style="color: #0000ff;">new</span><span style="color: #000000;"> ExpandoObject();
            table.TableA </span>=<span style="color: #000000;"> tableA();
            table.TableB </span>=<span style="color: #000000;"> tableB();
            </span><span style="color: #0000ff;">return</span><span style="color: #000000;"> View(table);
        }</span></pre>
</div>
<div class="cnblogs_code">
<pre><span style="color: #000000;"><span style="background-color: #ffff00;">@model dynamic</span>
@{
    Layout = null;
}
</span><span style="color: #0000ff;">&lt;!</span><span style="color: #ff00ff;">DOCTYPE html</span><span style="color: #0000ff;">&gt;</span>
<span style="color: #0000ff;">&lt;</span><span style="color: #800000;">html</span><span style="color: #0000ff;">&gt;</span>
<span style="color: #0000ff;">&lt;</span><span style="color: #800000;">head</span><span style="color: #0000ff;">&gt;</span>
    <span style="color: #0000ff;">&lt;</span><span style="color: #800000;">title</span><span style="color: #0000ff;">&gt;</span>Test<span style="color: #0000ff;">&lt;/</span><span style="color: #800000;">title</span><span style="color: #0000ff;">&gt;</span>
<span style="color: #0000ff;">&lt;/</span><span style="color: #800000;">head</span><span style="color: #0000ff;">&gt;</span>
<span style="color: #0000ff;">&lt;</span><span style="color: #800000;">body</span><span style="color: #0000ff;">&gt;</span>
    <span style="color: #0000ff;">&lt;</span><span style="color: #800000;">div</span><span style="color: #0000ff;">&gt;</span>
        <span style="color: #0000ff;">&lt;</span><span style="color: #800000;">table </span><span style="color: #ff0000;">class</span><span style="color: #0000ff;">="table1"</span><span style="color: #0000ff;">&gt;</span>
        <span style="color: #0000ff;">&lt;</span><span style="color: #800000;">tbody</span><span style="color: #0000ff;">&gt;</span><span style="color: #000000;">
        @foreach (var item in Model.TableA)
        {
            </span><span style="color: #0000ff;">&lt;</span><span style="color: #800000;">tr</span><span style="color: #0000ff;">&gt;</span>
                <span style="color: #0000ff;">&lt;</span><span style="color: #800000;">td</span><span style="color: #0000ff;">&gt;</span>@item.A<span style="color: #0000ff;">&lt;/</span><span style="color: #800000;">td</span><span style="color: #0000ff;">&gt;</span>
                <span style="color: #0000ff;">&lt;</span><span style="color: #800000;">td</span><span style="color: #0000ff;">&gt;</span>@item.B<span style="color: #0000ff;">&lt;/</span><span style="color: #800000;">td</span><span style="color: #0000ff;">&gt;</span>
                <span style="color: #0000ff;">&lt;</span><span style="color: #800000;">td</span><span style="color: #0000ff;">&gt;</span>@item.C<span style="color: #0000ff;">&lt;/</span><span style="color: #800000;">td</span><span style="color: #0000ff;">&gt;</span>
            <span style="color: #0000ff;">&lt;/</span><span style="color: #800000;">tr</span><span style="color: #0000ff;">&gt;</span><span style="color: #000000;">
        }
        </span><span style="color: #0000ff;">&lt;/</span><span style="color: #800000;">tbody</span><span style="color: #0000ff;">&gt;</span>
    <span style="color: #0000ff;">&lt;/</span><span style="color: #800000;">table</span><span style="color: #0000ff;">&gt;</span>
    <span style="color: #0000ff;">&lt;</span><span style="color: #800000;">table </span><span style="color: #ff0000;">class</span><span style="color: #0000ff;">="table2"</span><span style="color: #0000ff;">&gt;</span>
        <span style="color: #0000ff;">&lt;</span><span style="color: #800000;">tbody</span><span style="color: #0000ff;">&gt;</span><span style="color: #000000;">
        @foreach (var item in Model.TableB)
        {
            </span><span style="color: #0000ff;">&lt;</span><span style="color: #800000;">tr</span><span style="color: #0000ff;">&gt;</span>
                <span style="color: #0000ff;">&lt;</span><span style="color: #800000;">td</span><span style="color: #0000ff;">&gt;</span>@item.X<span style="color: #0000ff;">&lt;/</span><span style="color: #800000;">td</span><span style="color: #0000ff;">&gt;</span>
                <span style="color: #0000ff;">&lt;</span><span style="color: #800000;">td</span><span style="color: #0000ff;">&gt;</span>@item.Y<span style="color: #0000ff;">&lt;/</span><span style="color: #800000;">td</span><span style="color: #0000ff;">&gt;</span>
                <span style="color: #0000ff;">&lt;</span><span style="color: #800000;">td</span><span style="color: #0000ff;">&gt;</span>@item.Z<span style="color: #0000ff;">&lt;/</span><span style="color: #800000;">td</span><span style="color: #0000ff;">&gt;</span>
            <span style="color: #0000ff;">&lt;/</span><span style="color: #800000;">tr</span><span style="color: #0000ff;">&gt;</span><span style="color: #000000;">
        }
        </span><span style="color: #0000ff;">&lt;/</span><span style="color: #800000;">tbody</span><span style="color: #0000ff;">&gt;</span>
    <span style="color: #0000ff;">&lt;/</span><span style="color: #800000;">table</span><span style="color: #0000ff;">&gt;</span>
    <span style="color: #0000ff;">&lt;/</span><span style="color: #800000;">div</span><span style="color: #0000ff;">&gt;</span>
<span style="color: #0000ff;">&lt;/</span><span style="color: #800000;">body</span><span style="color: #0000ff;">&gt;</span>
<span style="color: #0000ff;">&lt;/</span><span style="color: #800000;">html</span><span style="color: #0000ff;">&gt;</span></pre>
</div>
<p><strong>方法三：使用Tuple传递数据</strong></p>
<div class="cnblogs_code">
<pre>        <span style="color: #0000ff;">public</span><span style="color: #000000;"> ActionResult Tuple()
        {
            </span><span style="color: #0000ff;">var</span> table1 =<span style="color: #000000;"> tableA();
            </span><span style="color: #0000ff;">var</span> table2 =<span style="color: #000000;"> tableB();
            </span><span style="color: #0000ff;">var</span> TupleModel = <span style="color: #0000ff;">new</span> Tuple&lt;List&lt;TableA&gt;, List&lt;TableB&gt;&gt;<span style="color: #000000;">(table1, table2);
            </span><span style="color: #0000ff;">return</span><span style="color: #000000;"> View(TupleModel);
        }</span></pre>
</div>
<div class="cnblogs_code">
<pre><span style="background-color: #ffff00;"><span style="color: #000000;">@using ViewModelStudy.Models;
@model Tuple</span><span style="color: #0000ff;">&lt;</span><span style="color: #800000;">List</span><span style="color: #ff0000;">&lt;TableA</span><span style="color: #0000ff;">&gt;</span>,List<span style="color: #0000ff;">&lt;</span><span style="color: #800000;">TableB</span><span style="color: #0000ff;">&gt;</span></span><span style="color: #000000;"><span style="background-color: #ffff00;">&gt;</span>
@{
    Layout = null;
}

</span><span style="color: #0000ff;">&lt;!</span><span style="color: #ff00ff;">DOCTYPE html</span><span style="color: #0000ff;">&gt;</span>

<span style="color: #0000ff;">&lt;</span><span style="color: #800000;">html</span><span style="color: #0000ff;">&gt;</span>
<span style="color: #0000ff;">&lt;</span><span style="color: #800000;">head</span><span style="color: #0000ff;">&gt;</span>
    <span style="color: #0000ff;">&lt;</span><span style="color: #800000;">title</span><span style="color: #0000ff;">&gt;</span>Tuple<span style="color: #0000ff;">&lt;/</span><span style="color: #800000;">title</span><span style="color: #0000ff;">&gt;</span>
<span style="color: #0000ff;">&lt;/</span><span style="color: #800000;">head</span><span style="color: #0000ff;">&gt;</span>
<span style="color: #0000ff;">&lt;</span><span style="color: #800000;">body</span><span style="color: #0000ff;">&gt;</span>
    <span style="color: #0000ff;">&lt;</span><span style="color: #800000;">div</span><span style="color: #0000ff;">&gt;</span>
        <span style="color: #0000ff;">&lt;</span><span style="color: #800000;">table </span><span style="color: #ff0000;">class</span><span style="color: #0000ff;">="table1"</span><span style="color: #0000ff;">&gt;</span>
        <span style="color: #0000ff;">&lt;</span><span style="color: #800000;">tbody</span><span style="color: #0000ff;">&gt;</span><span style="color: #000000;">
        @foreach (var item in Model.Item1)
        {
            </span><span style="color: #0000ff;">&lt;</span><span style="color: #800000;">tr</span><span style="color: #0000ff;">&gt;</span>
                <span style="color: #0000ff;">&lt;</span><span style="color: #800000;">td</span><span style="color: #0000ff;">&gt;</span>@item.A<span style="color: #0000ff;">&lt;/</span><span style="color: #800000;">td</span><span style="color: #0000ff;">&gt;</span>
                <span style="color: #0000ff;">&lt;</span><span style="color: #800000;">td</span><span style="color: #0000ff;">&gt;</span>@item.B<span style="color: #0000ff;">&lt;/</span><span style="color: #800000;">td</span><span style="color: #0000ff;">&gt;</span>
                <span style="color: #0000ff;">&lt;</span><span style="color: #800000;">td</span><span style="color: #0000ff;">&gt;</span>@item.C<span style="color: #0000ff;">&lt;/</span><span style="color: #800000;">td</span><span style="color: #0000ff;">&gt;</span>
            <span style="color: #0000ff;">&lt;/</span><span style="color: #800000;">tr</span><span style="color: #0000ff;">&gt;</span><span style="color: #000000;">
        }
        </span><span style="color: #0000ff;">&lt;/</span><span style="color: #800000;">tbody</span><span style="color: #0000ff;">&gt;</span>
    <span style="color: #0000ff;">&lt;/</span><span style="color: #800000;">table</span><span style="color: #0000ff;">&gt;</span>
    <span style="color: #0000ff;">&lt;</span><span style="color: #800000;">h1</span><span style="color: #0000ff;">&gt;</span>xxxxxxxxxxxxxxxxxxx<span style="color: #0000ff;">&lt;/</span><span style="color: #800000;">h1</span><span style="color: #0000ff;">&gt;</span>
    <span style="color: #0000ff;">&lt;</span><span style="color: #800000;">table </span><span style="color: #ff0000;">class</span><span style="color: #0000ff;">="table2"</span><span style="color: #0000ff;">&gt;</span>
        <span style="color: #0000ff;">&lt;</span><span style="color: #800000;">tbody</span><span style="color: #0000ff;">&gt;</span><span style="color: #000000;">
        @foreach (var item in Model.Item2)
        {
            </span><span style="color: #0000ff;">&lt;</span><span style="color: #800000;">tr</span><span style="color: #0000ff;">&gt;</span>
                <span style="color: #0000ff;">&lt;</span><span style="color: #800000;">td</span><span style="color: #0000ff;">&gt;</span>@item.X<span style="color: #0000ff;">&lt;/</span><span style="color: #800000;">td</span><span style="color: #0000ff;">&gt;</span>
                <span style="color: #0000ff;">&lt;</span><span style="color: #800000;">td</span><span style="color: #0000ff;">&gt;</span>@item.Y<span style="color: #0000ff;">&lt;/</span><span style="color: #800000;">td</span><span style="color: #0000ff;">&gt;</span>
                <span style="color: #0000ff;">&lt;</span><span style="color: #800000;">td</span><span style="color: #0000ff;">&gt;</span>@item.Z<span style="color: #0000ff;">&lt;/</span><span style="color: #800000;">td</span><span style="color: #0000ff;">&gt;</span>
            <span style="color: #0000ff;">&lt;/</span><span style="color: #800000;">tr</span><span style="color: #0000ff;">&gt;</span><span style="color: #000000;">
        }
        </span><span style="color: #0000ff;">&lt;/</span><span style="color: #800000;">tbody</span><span style="color: #0000ff;">&gt;</span>
    <span style="color: #0000ff;">&lt;/</span><span style="color: #800000;">table</span><span style="color: #0000ff;">&gt;</span>
    <span style="color: #0000ff;">&lt;/</span><span style="color: #800000;">div</span><span style="color: #0000ff;">&gt;</span>
<span style="color: #0000ff;">&lt;/</span><span style="color: #800000;">body</span><span style="color: #0000ff;">&gt;</span>
<span style="color: #0000ff;">&lt;/</span><span style="color: #800000;">html</span><span style="color: #0000ff;">&gt;</span></pre>
</div>
<p><strong>总结</strong></p>
<p><strong>　　使用新建视图模型优点在于对于较为复杂集合展示数据时，使用强类型能够较方便找到集合下面的实体属性，而缺点在于需要新建实体类，可能有相当一部分人都不喜欢新建实体类。</strong></p>
<p><strong>　　使用动态类型和新疆视图模型相比，优势在于不需要新建实体类，想怎么命名就怎么命名，缺点也是由此而来，没法动态推断出集合下的实体类属性，可能对于集合属性比较复杂的页面来说单单敲出这些属性就是一个很大的问题。</strong></p>
<p><strong>　　Tuple传递数据是我比较喜欢的一种方式，你只需要记住该集合中各部分数据的序号即可，而且对于实体类可以动态给出其包含的属性。</strong></p>
<p>&nbsp;</p>