<p>1.Table详情页跳转：直接使用<strong><span style="color: #0000ff;">HtmlHelper.ActionLink</span></strong>,与在Bootstrap中不同的是：需要增加target属性，否则，点击后会在新标签页打开</p>
<div class="cnblogs_code">
<pre><span style="color: #0000ff;">public</span> <span style="color: #0000ff;">static</span> MvcHtmlString ActionLink(<span style="color: #0000ff;">this</span> HtmlHelper htmlHelper, <span style="color: #0000ff;">string</span> linkText, <span style="color: #0000ff;">string</span> actionName, <span style="color: #0000ff;">object</span> routeValues, <span style="color: #0000ff;">object</span><span style="color: #000000;"> htmlAttributes);
</span><span style="color: #008000;">//</span><span style="color: #008000;">Example</span>
Html.ActionLink(<span style="color: #800000;">"</span><span style="color: #800000;">Detail</span><span style="color: #800000;">"</span>,<span style="color: #800000;">"</span><span style="color: #800000;">Detail</span><span style="color: #800000;">"</span>, <span style="color: #0000ff;">new</span>{ id=@item.ID }, <span style="color: #0000ff;">new</span>{ target=<span style="color: #800000;">"</span><span style="color: #800000;">navTab</span><span style="color: #800000;">"</span> });</pre>
</div>
<p>学习资料：<a title="Html.ActionLink" href="http://www.cnblogs.com/Relict/archive/2012/04/27/2473340.html" target="_blank">http://www.cnblogs.com/Relict/archive/2012/04/27/2473340.html&nbsp;</a></p>
<p>2.页面显示方式：</p>
<p>　　<strong><span style="color: #0000ff;">navTab</span></strong>：标签页显示画面</p>
<p>　　<span style="color: #0000ff;"><strong>dialog</strong></span>：弹出式显示画面</p>
<p>3.搭建流程：</p>
<p>　　1） 访问DWZ框架官网<a href="http://jui.org/" target="_blank">http://jui.org/</a>，通过oschina或github下载源码。</p>
<p>　　2） &nbsp;将dwz_jui\bin\dwz.min.js，dwz_jui\js，dwz_jui\themes\css，dwz_jui\dwz.frag.xml，dwz_jui\uploadify导入到项目目录下。</p>
<p>　　3） &nbsp;引入必须的js文件及css文件，搭建基础模板。</p>