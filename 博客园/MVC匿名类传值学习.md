<p>　　刚接触MVC+EF框架不久，但一直很困惑的就是控制器能否及如何向视图传递匿名类数据。宝宝表示很讨厌去新建实体类啦，查询稍有不同就去建一个实体类不是很麻烦吗，故趁阳光正好，周末睡到自然醒后起来尝试了之前一直在博客园看到的实现方式：英明神武的Tuple类，第一次对微软钦佩之至。故做如下记录，方便自己之后使用。大神就勿喷我啦，宝宝第一次写博客。</p>
<p>　　首先先描述一下我要实现的功能：<span style="color: #0000ff;">从控制器后台查询一些数据，通过匿名类存储，在视图前端遍历输出</span>。初衷实现流程如下：</p>
<p>控制器部分：</p>
<div class="cnblogs_Highlighter">
<pre class="brush:csharp;gutter:true;">      private repairsystemEntities db = new repairsystemEntities();
        // GET: TEST
        public ActionResult Index()
        {
            var Info = db.bom.ToList().Select(p =&gt; <span style="color: #ff0000;">Tuple.Create(p.Bom_Brand, p.Bom_Model)</span>);
            ViewBag.Info = Info;
            return View();
        }　</pre>
</div>
<p>视图部分：</p>
<div class="cnblogs_code">
<pre><span style="color: #0000ff;">&lt;</span><span style="color: #800000;">table </span><span style="color: #ff0000;">class</span><span style="color: #0000ff;">="table table-hover"</span><span style="color: #0000ff;">&gt;</span>
    <span style="color: #0000ff;">&lt;</span><span style="color: #800000;">tbody</span><span style="color: #0000ff;">&gt;</span><span style="color: #000000;">
    @foreach(var item in ViewBag.Info)
    {
        </span><span style="color: #0000ff;">&lt;</span><span style="color: #800000;">tr</span><span style="color: #0000ff;">&gt;</span>
            <span style="color: #0000ff;">&lt;</span><span style="color: #800000;">td</span><span style="color: #0000ff;">&gt;</span><span style="color: #ff0000;">@(item.Item1)</span><span style="color: #0000ff;">&lt;/</span><span style="color: #800000;">td</span><span style="color: #0000ff;">&gt;</span>
        <span style="color: #0000ff;">&lt;/</span><span style="color: #800000;">tr</span><span style="color: #0000ff;">&gt;</span><span style="color: #000000;">
    }
    </span><span style="color: #0000ff;">&lt;/</span><span style="color: #800000;">tbody</span><span style="color: #0000ff;">&gt;</span>
<span style="color: #0000ff;">&lt;/</span><span style="color: #800000;">table</span><span style="color: #0000ff;">&gt;</span></pre>
</div>
<p>附Tuple类简单说明如下，全部来源于微软官方文档，<a title="微软官方Tuple类文档" href="https://msdn.microsoft.com/zh-cn/library/system.tuple.aspx">地址</a></p>
<h3><strong>语法</strong></h3>
<div class="cnblogs_Highlighter">
<pre class="brush:csharp;gutter:true;">public static Tuple&lt;T1&gt; Create&lt;T1&gt;(
	T1 item1
)　</pre>
</div>
<h5 class="subHeading">　　参数</h5>
<p><span class="parameter" style="font-size: 14px;">　　item1</span></p>
<dl><dd>
<p><span style="font-size: 14px;">　　Type:&nbsp;<span class="selflink"><span class="selflink">T1</span></span></span></p>
<p><span id="mt3" class="sentence" style="font-size: 14px;" data-guid="8cb5b82aa7eebb00a6bfd0d64183c3c3" data-source="The value of the only component of the tuple.">　　元组仅有的分量的值。</span></p>
</dd></dl>
<h5 class="subHeading">　　返回值</h5>
<p>　　Type:&nbsp;<span><span>System.Tuple&lt;<span class="selflink">T1&gt;</span></span></span></p>
<p><span id="mt4" class="sentence" data-guid="45c055062dc499a9f00611e492c5fe0c" data-source="A tuple whose value is (&lt;em&gt;item1&lt;/em&gt;).">　　元组，其值为 (<em>item1</em>)</span></p>
<h3><span class="sentence" data-guid="45c055062dc499a9f00611e492c5fe0c" data-source="A tuple whose value is (&lt;em&gt;item1&lt;/em&gt;).">使用方法</span></h3>
<div class="cnblogs_code">
<pre><span style="color: #008000;">//</span><span style="color: #008000;">类构造函数</span>
<span style="color: #0000ff;">var</span> tuple1 = <span style="color: #0000ff;">new</span> Tuple&lt;<span style="color: #0000ff;">int</span>&gt;(<span style="color: #800080;">12</span><span style="color: #000000;">);
</span><span style="color: #008000;">//</span><span style="color: #008000;">helper方法</span>
<span style="color: #0000ff;">var</span> tuple2 = Tuple.Create(<span style="color: #800080;">12</span><span style="color: #000000;">);
</span><span style="color: #008000;">//</span><span style="color: #008000;">获取值方法直接采用</span>
Console.WriteLine(tuple1.Item1);     <span style="color: #008000;">//</span><span style="color: #008000;"> Displays 12</span>
Console.WriteLine(tuple2.Item1);     <span style="color: #008000;">//</span><span style="color: #008000;"> Displays 12</span></pre>
</div>
<h3>实际例子</h3>
<div class="cnblogs_code" onclick="cnblogs_code_show('49198a25-a9fd-4178-8679-5e81c3d03493')"><img id="code_img_closed_49198a25-a9fd-4178-8679-5e81c3d03493" class="code_img_closed" src="http://images.cnblogs.com/OutliningIndicators/ContractedBlock.gif" alt="" /><img id="code_img_opened_49198a25-a9fd-4178-8679-5e81c3d03493" class="code_img_opened" style="display: none;" onclick="cnblogs_code_hide('49198a25-a9fd-4178-8679-5e81c3d03493',event)" src="http://images.cnblogs.com/OutliningIndicators/ExpandedBlockStart.gif" alt="" />
<div id="cnblogs_code_open_49198a25-a9fd-4178-8679-5e81c3d03493" class="cnblogs_code_hide">
<pre><span style="color: #008000;">//</span><span style="color: #008000;"> Create a 7-tuple.</span>
<span style="color: #0000ff;">var</span> population = <span style="color: #0000ff;">new</span> Tuple&lt;<span style="color: #0000ff;">string</span>, <span style="color: #0000ff;">int</span>, <span style="color: #0000ff;">int</span>, <span style="color: #0000ff;">int</span>, <span style="color: #0000ff;">int</span>, <span style="color: #0000ff;">int</span>, <span style="color: #0000ff;">int</span>&gt;<span style="color: #000000;">(
                           </span><span style="color: #800000;">"</span><span style="color: #800000;">New York</span><span style="color: #800000;">"</span>, <span style="color: #800080;">7891957</span>, <span style="color: #800080;">7781984</span><span style="color: #000000;">, 
                           </span><span style="color: #800080;">7894862</span>, <span style="color: #800080;">7071639</span>, <span style="color: #800080;">7322564</span>, <span style="color: #800080;">8008278</span><span style="color: #000000;">);
</span><span style="color: #008000;">//</span><span style="color: #008000;"> Display the first and last elements.</span>
Console.WriteLine(<span style="color: #800000;">"</span><span style="color: #800000;">Population of {0} in 2000: {1:N0}</span><span style="color: #800000;">"</span><span style="color: #000000;">,
                  population.Item1, population.Item7);
</span><span style="color: #008000;">//</span><span style="color: #008000;"> The example displays the following output:
</span><span style="color: #008000;">//</span><span style="color: #008000;">       Population of New York in 2000: 8,008,278</span></pre>
</div>
<span class="cnblogs_code_collapse">类构造函数创建</span></div>
<div class="cnblogs_code" onclick="cnblogs_code_show('f336e888-3e1d-44d2-a7bf-23204769dff8')"><img id="code_img_closed_f336e888-3e1d-44d2-a7bf-23204769dff8" class="code_img_closed" src="http://images.cnblogs.com/OutliningIndicators/ContractedBlock.gif" alt="" /><img id="code_img_opened_f336e888-3e1d-44d2-a7bf-23204769dff8" class="code_img_opened" style="display: none;" onclick="cnblogs_code_hide('f336e888-3e1d-44d2-a7bf-23204769dff8',event)" src="http://images.cnblogs.com/OutliningIndicators/ExpandedBlockStart.gif" alt="" />
<div id="cnblogs_code_open_f336e888-3e1d-44d2-a7bf-23204769dff8" class="cnblogs_code_hide">
<pre><span style="color: #008000;">//</span><span style="color: #008000;"> Create a 7-tuple.</span>
<span style="color: #0000ff;">var</span> population = Tuple.Create(<span style="color: #800000;">"</span><span style="color: #800000;">New York</span><span style="color: #800000;">"</span>, <span style="color: #800080;">7891957</span>, <span style="color: #800080;">7781984</span>, <span style="color: #800080;">7894862</span>, <span style="color: #800080;">7071639</span>, <span style="color: #800080;">7322564</span>, <span style="color: #800080;">8008278</span><span style="color: #000000;">);
</span><span style="color: #008000;">//</span><span style="color: #008000;"> Display the first and last elements.</span>
Console.WriteLine(<span style="color: #800000;">"</span><span style="color: #800000;">Population of {0} in 2000: {1:N0}</span><span style="color: #800000;">"</span><span style="color: #000000;">,
                  population.Item1, population.Item7);
</span><span style="color: #008000;">//</span><span style="color: #008000;"> The example displays the following output:
</span><span style="color: #008000;">//</span><span style="color: #008000;">       Population of New York in 2000: 8,008,278</span></pre>
</div>
<span class="cnblogs_code_collapse">Create方法</span></div>
<p>&nbsp;</p>