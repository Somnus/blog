<p>刚接触MVC不久，写的一些代码自己都不忍心看下去。路漫漫其修远兮，宝宝还需努力！之前只用过Session做登录时用户信息的储存，今天对集合类数据做了小小的尝试：利用session在控制器之间传值，以减少代重复率。</p>
<p>1.将数据储存到Session中（不受类型限制）；</p>
<p>2.从session中读取数据（注意转换为正确的的数据类型）；</p>
<p>3.随你怎么操作。</p>
<div class="cnblogs_code">
<pre><span style="color: #0000ff;">using</span><span style="color: #000000;"> System.Collections.Generic;
</span><span style="color: #0000ff;">using</span><span style="color: #000000;"> System.Linq;
</span><span style="color: #0000ff;">using</span><span style="color: #000000;"> System.Web.Mvc;

</span><span style="color: #0000ff;">namespace</span><span style="color: #000000;"> TempDataStudy.Controllers
{<br />　　//定义一个实体类
    </span><span style="color: #0000ff;">public</span> <span style="color: #0000ff;">class</span><span style="color: #000000;"> Stu
    {
        </span><span style="color: #0000ff;">public</span> <span style="color: #0000ff;">int</span> Id { <span style="color: #0000ff;">get</span>; <span style="color: #0000ff;">set</span><span style="color: #000000;">; }
        </span><span style="color: #0000ff;">public</span> <span style="color: #0000ff;">string</span> Name { <span style="color: #0000ff;">get</span>; <span style="color: #0000ff;">set</span><span style="color: #000000;">; }
        </span><span style="color: #0000ff;">public</span> <span style="color: #0000ff;">int</span> Age { <span style="color: #0000ff;">get</span>; <span style="color: #0000ff;">set</span><span style="color: #000000;">; }
    }
    </span><span style="color: #0000ff;">public</span> <span style="color: #0000ff;">class</span><span style="color: #000000;"> HomeController : Controller
    {    
        </span><span style="color: #0000ff;">public</span><span style="color: #000000;"> ActionResult Index()
        {
            List</span>&lt;Stu&gt; Student = <span style="color: #0000ff;">new</span> List&lt;Stu&gt;<span style="color: #000000;">
            {
                </span><span style="color: #0000ff;">new</span> Stu{Id=<span style="color: #800080;">1</span>,Name=<span style="color: #800000;">"</span><span style="color: #800000;">张思宁</span><span style="color: #800000;">"</span>,Age=<span style="color: #800080;">22</span><span style="color: #000000;">},
                </span><span style="color: #0000ff;">new</span> Stu{Id=<span style="color: #800080;">2</span>,Name=<span style="color: #800000;">"</span><span style="color: #800000;">习1近平</span><span style="color: #800000;">"</span>,Age=<span style="color: #800080;">24</span><span style="color: #000000;">},
                </span><span style="color: #0000ff;">new</span> Stu{Id=<span style="color: #800080;">3</span>,Name=<span style="color: #800000;">"</span><span style="color: #800000;">江1泽民</span><span style="color: #800000;">"</span>,Age=<span style="color: #800080;">20</span><span style="color: #000000;">}
            };
            ViewBag.Student </span>= Student;<span style="color: #008000;">//</span><span style="color: #008000;">向视图传值<br /></span></pre>
<pre><span>            Session.Remove("Stu");//移除Session["Stu"]</span></pre>
<pre>            Session[<span style="color: #800000;">"</span><span style="color: #800000;">Stu</span><span style="color: #800000;">"</span>] = Student;<span style="color: #008000;">//</span><span style="color: #008000;">向控制器About传值</span>
            <span style="color: #0000ff;">return</span><span style="color: #000000;"> View();
        }
        </span><span style="color: #0000ff;">public</span> ActionResult About(<span style="color: #0000ff;">int</span><span style="color: #000000;"> id)
        {
            </span><span style="color: #0000ff;">var</span> Student = Session[<span style="color: #800000;">"</span><span style="color: #800000;">Stu</span><span style="color: #800000;">"</span>] <span style="color: #0000ff;">as</span> List&lt;Stu&gt;;<span style="color: #008000;">//</span><span style="color: #008000;">获取存储在Session中的值</span>
            ViewBag.StuInfo = Student.Where(p =&gt; p.Id ==<span style="color: #000000;"> id).FirstOrDefault();</span>
            <span style="color: #0000ff;">return</span><span style="color: #000000;"> View();
        }
    }
}</span></pre>
</div>
<div class="cnblogs_Highlighter">
<pre class="brush:html;gutter:true;">@{
    ViewBag.Title = "Home Page";
}
&lt;table class="table table-hover"&gt;
    &lt;thead&gt;
        &lt;tr&gt;
            &lt;th&gt;序号&lt;/th&gt;
            &lt;th&gt;姓名&lt;/th&gt;
            &lt;th&gt;操作&lt;/th&gt;
        &lt;/tr&gt;
    &lt;/thead&gt;
    &lt;tbody&gt;
        @foreach (var item in ViewBag.Student)
        {
            &lt;tr&gt;
                &lt;td&gt;@item.Id&lt;/td&gt;
                &lt;td&gt;@item.Name&lt;/td&gt;
                &lt;td&gt;@Html.ActionLink("详情", "About",new { id = item.Id })&lt;/td&gt;
            &lt;/tr&gt;
        }
    &lt;/tbody&gt;
&lt;/table&gt;
</pre>
</div>
<p>　　</p>
<div class="cnblogs_Highlighter">
<pre class="brush:html;gutter:true;">@{ 
    ViewBag.Title = "About";
    var t = ViewBag.StuInfo;
}
序号：@t.Id
姓名：@t.Name
年龄：@t.Age
</pre>
</div>
<p>　　运行结果：</p>
<p><img src="http://images2017.cnblogs.com/blog/1159701/201707/1159701-20170726223010937-243252603.png" alt="" /></p>
<p>&nbsp;</p>
<p><img src="http://images2017.cnblogs.com/blog/1159701/201707/1159701-20170726222938859-749742615.png" alt="" /></p>
<p>&nbsp;</p>