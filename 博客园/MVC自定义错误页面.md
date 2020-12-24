<p>　　MVC异常处理主要有三种方案：1.基于HandleErrorAttribute重写OnException方法；2.基于Global.apsx添加Application_Error方法；3.直接在Web.Config中配置。现基于上述思路，测试了下面三种自定义错误页面的处理方法（主要侧重于显示异常信息，便于快速找到代码中的异常来源），以便后续查阅。不足之处，还请指教！</p>
<p>1.直接在web.config的&lt;system.web&gt;节点下加入&lt;customErrors mode="On" /&gt;，在View/Shared/Error.cshtml中写入异常信息，发生异常时会跳转到该l页面。</p>
<div class="cnblogs_code" onclick="cnblogs_code_show('78222143-a46b-4c54-9e63-c23874e46429')"><img id="code_img_closed_78222143-a46b-4c54-9e63-c23874e46429" class="code_img_closed" src="http://images.cnblogs.com/OutliningIndicators/ContractedBlock.gif" alt="" /><img id="code_img_opened_78222143-a46b-4c54-9e63-c23874e46429" class="code_img_opened" style="display: none;" onclick="cnblogs_code_hide('78222143-a46b-4c54-9e63-c23874e46429',event)" src="http://images.cnblogs.com/OutliningIndicators/ExpandedBlockStart.gif" alt="" />
<div id="cnblogs_code_open_78222143-a46b-4c54-9e63-c23874e46429" class="cnblogs_code_hide">
<pre>  &lt;system.web&gt;
    &lt;customErrors mode=<span style="color: #800000;">"</span><span style="color: #800000;">On</span><span style="color: #800000;">"</span> /&gt;
    &lt;compilation debug=<span style="color: #800000;">"</span><span style="color: #800000;">true</span><span style="color: #800000;">"</span> targetFramework=<span style="color: #800000;">"</span><span style="color: #800000;">4.0</span><span style="color: #800000;">"</span>&gt;
      &lt;assemblies&gt;
        &lt;add assembly=<span style="color: #800000;">"</span><span style="color: #800000;">System.Web.Abstractions, Version=4.0.0.0, Culture=neutral, PublicKeyToken=31BF3856AD364E35</span><span style="color: #800000;">"</span> /&gt;
        &lt;add assembly=<span style="color: #800000;">"</span><span style="color: #800000;">System.Web.Helpers, Version=1.0.0.0, Culture=neutral, PublicKeyToken=31BF3856AD364E35</span><span style="color: #800000;">"</span> /&gt;
        &lt;add assembly=<span style="color: #800000;">"</span><span style="color: #800000;">System.Web.Routing, Version=4.0.0.0, Culture=neutral, PublicKeyToken=31BF3856AD364E35</span><span style="color: #800000;">"</span> /&gt;
        &lt;add assembly=<span style="color: #800000;">"</span><span style="color: #800000;">System.Web.Mvc, Version=3.0.0.0, Culture=neutral, PublicKeyToken=31BF3856AD364E35</span><span style="color: #800000;">"</span> /&gt;
        &lt;add assembly=<span style="color: #800000;">"</span><span style="color: #800000;">System.Web.WebPages, Version=1.0.0.0, Culture=neutral, PublicKeyToken=31BF3856AD364E35</span><span style="color: #800000;">"</span> /&gt;
      &lt;/assemblies&gt;
    &lt;/compilation&gt;
    &lt;authentication mode=<span style="color: #800000;">"</span><span style="color: #800000;">Forms</span><span style="color: #800000;">"</span>&gt;
      &lt;forms loginUrl=<span style="color: #800000;">"</span><span style="color: #800000;">~/Account/LogOn</span><span style="color: #800000;">"</span> timeout=<span style="color: #800000;">"</span><span style="color: #800000;">2880</span><span style="color: #800000;">"</span> /&gt;
    &lt;/authentication&gt;
    &lt;pages&gt;
      &lt;namespaces&gt;
        &lt;add <span style="color: #0000ff;">namespace</span>=<span style="color: #800000;">"</span><span style="color: #800000;">System.Web.Helpers</span><span style="color: #800000;">"</span> /&gt;
        &lt;add <span style="color: #0000ff;">namespace</span>=<span style="color: #800000;">"</span><span style="color: #800000;">System.Web.Mvc</span><span style="color: #800000;">"</span> /&gt;
        &lt;add <span style="color: #0000ff;">namespace</span>=<span style="color: #800000;">"</span><span style="color: #800000;">System.Web.Mvc.Ajax</span><span style="color: #800000;">"</span> /&gt;
        &lt;add <span style="color: #0000ff;">namespace</span>=<span style="color: #800000;">"</span><span style="color: #800000;">System.Web.Mvc.Html</span><span style="color: #800000;">"</span> /&gt;
        &lt;add <span style="color: #0000ff;">namespace</span>=<span style="color: #800000;">"</span><span style="color: #800000;">System.Web.Routing</span><span style="color: #800000;">"</span> /&gt;
        &lt;add <span style="color: #0000ff;">namespace</span>=<span style="color: #800000;">"</span><span style="color: #800000;">System.Web.WebPages</span><span style="color: #800000;">"</span>/&gt;
      &lt;/namespaces&gt;
    &lt;/pages&gt;
  &lt;/system.web&gt;</pre>
</div>
<span class="cnblogs_code_collapse">Web.Config</span></div>
<div class="cnblogs_code" onclick="cnblogs_code_show('7bb56db8-3e4e-4a75-ae2b-7b023226b3e2')"><img id="code_img_closed_7bb56db8-3e4e-4a75-ae2b-7b023226b3e2" class="code_img_closed" src="http://images.cnblogs.com/OutliningIndicators/ContractedBlock.gif" alt="" /><img id="code_img_opened_7bb56db8-3e4e-4a75-ae2b-7b023226b3e2" class="code_img_opened" style="display: none;" onclick="cnblogs_code_hide('7bb56db8-3e4e-4a75-ae2b-7b023226b3e2',event)" src="http://images.cnblogs.com/OutliningIndicators/ExpandedBlockStart.gif" alt="" />
<div id="cnblogs_code_open_7bb56db8-3e4e-4a75-ae2b-7b023226b3e2" class="cnblogs_code_hide">
<pre><span style="color: #000000;">@model HandleErrorInfo
@{
    Layout = null;
}

</span><span style="color: #0000ff;">&lt;!</span><span style="color: #ff00ff;">DOCTYPE html</span><span style="color: #0000ff;">&gt;</span>
<span style="color: #0000ff;">&lt;</span><span style="color: #800000;">html</span><span style="color: #0000ff;">&gt;</span>
<span style="color: #0000ff;">&lt;</span><span style="color: #800000;">head</span><span style="color: #0000ff;">&gt;</span>
    <span style="color: #0000ff;">&lt;</span><span style="color: #800000;">title</span><span style="color: #0000ff;">&gt;</span>Error<span style="color: #0000ff;">&lt;/</span><span style="color: #800000;">title</span><span style="color: #0000ff;">&gt;</span>
<span style="color: #0000ff;">&lt;/</span><span style="color: #800000;">head</span><span style="color: #0000ff;">&gt;</span>
<span style="color: #0000ff;">&lt;</span><span style="color: #800000;">body</span><span style="color: #0000ff;">&gt;</span>
    <span style="color: #0000ff;">&lt;</span><span style="color: #800000;">p</span><span style="color: #0000ff;">&gt;</span>Message: @Model.Exception.Message<span style="color: #0000ff;">&lt;/</span><span style="color: #800000;">p</span><span style="color: #0000ff;">&gt;</span>
    <span style="color: #0000ff;">&lt;</span><span style="color: #800000;">p</span><span style="color: #0000ff;">&gt;</span>Controller: @Model.ControllerName<span style="color: #0000ff;">&lt;/</span><span style="color: #800000;">p</span><span style="color: #0000ff;">&gt;</span>
    <span style="color: #0000ff;">&lt;</span><span style="color: #800000;">p</span><span style="color: #0000ff;">&gt;</span>Action: @Model.ActionName<span style="color: #0000ff;">&lt;/</span><span style="color: #800000;">p</span><span style="color: #0000ff;">&gt;</span>
    <span style="color: #0000ff;">&lt;</span><span style="color: #800000;">p</span><span style="color: #0000ff;">&gt;</span>Source: @Model.Exception.Source<span style="color: #0000ff;">&lt;/</span><span style="color: #800000;">p</span><span style="color: #0000ff;">&gt;</span>
    <span style="color: #0000ff;">&lt;</span><span style="color: #800000;">p</span><span style="color: #0000ff;">&gt;</span>Exception: @Model.Exception<span style="color: #0000ff;">&lt;/</span><span style="color: #800000;">p</span><span style="color: #0000ff;">&gt;</span>
<span style="color: #0000ff;">&lt;/</span><span style="color: #800000;">body</span><span style="color: #0000ff;">&gt;</span>
<span style="color: #0000ff;">&lt;/</span><span style="color: #800000;">html</span><span style="color: #0000ff;">&gt;</span></pre>
</div>
<span class="cnblogs_code_collapse">Error.html</span></div>
<p>2.新建类继承HandleErrorAttribute并重写OnException方法，将异常信息写入日志文件，并跳转到View/Shared/Error.cshtml错误页。直接将该属性作用于某控制器或者注册为全局变量以便于全局适用。</p>
<div class="cnblogs_code" onclick="cnblogs_code_show('a1e80613-dfc3-4031-96bd-3b33ce9a57fd')"><img id="code_img_closed_a1e80613-dfc3-4031-96bd-3b33ce9a57fd" class="code_img_closed" src="http://images.cnblogs.com/OutliningIndicators/ContractedBlock.gif" alt="" /><img id="code_img_opened_a1e80613-dfc3-4031-96bd-3b33ce9a57fd" class="code_img_opened" style="display: none;" onclick="cnblogs_code_hide('a1e80613-dfc3-4031-96bd-3b33ce9a57fd',event)" src="http://images.cnblogs.com/OutliningIndicators/ExpandedBlockStart.gif" alt="" />
<div id="cnblogs_code_open_a1e80613-dfc3-4031-96bd-3b33ce9a57fd" class="cnblogs_code_hide">
<pre><span style="color: #000000;">    [AttributeUsage(AttributeTargets.Class, Inherited = true, AllowMultiple = false)]
    public class LogExceptionAttribute : HandleErrorAttribute
    {
        public override void OnException(ExceptionContext filterContext)
        {
            string controllerName = (string)filterContext.RouteData.Values["controller"];
            string actionName = (string)filterContext.RouteData.Values["action"];
            HandleErrorInfo info = new HandleErrorInfo(filterContext.Exception, controllerName, actionName);

            Log log = new Log();
            log.Write(filterContext);

            if (!filterContext.ExceptionHandled)
            {

                filterContext.Result = new ViewResult() 
                {
                    ViewName = "/Views/Shared/Error.cshtml",
                    ViewData = new ViewDataDictionary</span><span style="color: #0000ff;">&lt;</span><span style="color: #800000;">HandleErrorInfo</span><span style="color: #0000ff;">&gt;</span><span style="color: #000000;">(info)
                };
                filterContext.ExceptionHandled = true;
                filterContext.HttpContext.Response.TrySkipIisCustomErrors = true;
            }
        }
    }</span></pre>
</div>
<span class="cnblogs_code_collapse">LogExceptionAttribute</span></div>
<p>错误页写入异常信息与方法1相同。</p>
<div class="cnblogs_code" onclick="cnblogs_code_show('a414787d-b638-4c9b-ab96-d47a677e2a79')"><img id="code_img_closed_a414787d-b638-4c9b-ab96-d47a677e2a79" class="code_img_closed" src="http://images.cnblogs.com/OutliningIndicators/ContractedBlock.gif" alt="" /><img id="code_img_opened_a414787d-b638-4c9b-ab96-d47a677e2a79" class="code_img_opened" style="display: none;" onclick="cnblogs_code_hide('a414787d-b638-4c9b-ab96-d47a677e2a79',event)" src="http://images.cnblogs.com/OutliningIndicators/ExpandedBlockStart.gif" alt="" />
<div id="cnblogs_code_open_a414787d-b638-4c9b-ab96-d47a677e2a79" class="cnblogs_code_hide">
<pre><span style="color: #000000;">   [LogException]
    </span><span style="color: #0000ff;">public</span> <span style="color: #0000ff;">class</span><span style="color: #000000;"> HomeController : Controller
    {
        </span><span style="color: #008000;">//</span>
        <span style="color: #008000;">//</span><span style="color: #008000;"> GET: /Home/</span>

        <span style="color: #0000ff;">public</span><span style="color: #000000;"> ActionResult Index()
        {
            </span><span style="color: #0000ff;">var</span> x=<span style="color: #800080;">1</span><span style="color: #000000;">;
            </span><span style="color: #0000ff;">string</span> y=<span style="color: #800000;">"</span><span style="color: #800000;">x</span><span style="color: #800000;">"</span><span style="color: #000000;">;
            ViewBag.t </span>= x /<span style="color: #000000;"> Convert.ToInt32(y);
            </span><span style="color: #0000ff;">return</span><span style="color: #000000;"> View();
        }
    }</span></pre>
</div>
<span class="cnblogs_code_collapse">作用于单一控制器</span></div>
<div class="cnblogs_code" onclick="cnblogs_code_show('aae9787a-a7a9-4f5a-a087-22974391fcb8')"><img id="code_img_closed_aae9787a-a7a9-4f5a-a087-22974391fcb8" class="code_img_closed" src="http://images.cnblogs.com/OutliningIndicators/ContractedBlock.gif" alt="" /><img id="code_img_opened_aae9787a-a7a9-4f5a-a087-22974391fcb8" class="code_img_opened" style="display: none;" onclick="cnblogs_code_hide('aae9787a-a7a9-4f5a-a087-22974391fcb8',event)" src="http://images.cnblogs.com/OutliningIndicators/ExpandedBlockStart.gif" alt="" />
<div id="cnblogs_code_open_aae9787a-a7a9-4f5a-a087-22974391fcb8" class="cnblogs_code_hide">
<pre><span style="color: #000000;"> public class MvcApplication : System.Web.HttpApplication
    {
        public static void RegisterGlobalFilters(GlobalFilterCollection filters)
        {
            filters.Add(new HandleErrorAttribute());
            //new 
            filters.Add(new LogExceptionAttribute());
        }

        public static void RegisterRoutes(RouteCollection routes)
        {
            routes.IgnoreRoute("{resource}.axd/{*pathInfo}");

            routes.MapRoute(
                "Default", // 路由名称
                "{controller}/{action}/{id}", // 带有参数的 URL
                new { controller = "Home", action = "Index", id = UrlParameter.Optional } // 参数默认值
            );

        }

        protected void Application_Start()
        {
            AreaRegistration.RegisterAllAreas();

            RegisterGlobalFilters(GlobalFilters.Filters);
            RegisterRoutes(RouteTable.Routes);
        }
    }</span></pre>
</div>
<span class="cnblogs_code_collapse">特性注册</span></div>
<p>3.新建继承于IController的基础控制器，重写OnException方法及写入异常日志文件，其他控制器只需要继承该基础控制器即可。与方法3的区别在于不需要注册全局筛选器取而代之的是控制器的继承。</p>
<div class="cnblogs_code" onclick="cnblogs_code_show('b2cc65e1-d2d1-4861-88bc-902aa69806a0')"><img id="code_img_closed_b2cc65e1-d2d1-4861-88bc-902aa69806a0" class="code_img_closed" src="http://images.cnblogs.com/OutliningIndicators/ContractedBlock.gif" alt="" /><img id="code_img_opened_b2cc65e1-d2d1-4861-88bc-902aa69806a0" class="code_img_opened" style="display: none;" onclick="cnblogs_code_hide('b2cc65e1-d2d1-4861-88bc-902aa69806a0',event)" src="http://images.cnblogs.com/OutliningIndicators/ExpandedBlockStart.gif" alt="" />
<div id="cnblogs_code_open_b2cc65e1-d2d1-4861-88bc-902aa69806a0" class="cnblogs_code_hide">
<pre><span style="color: #0000ff;">public</span> <span style="color: #0000ff;">class</span><span style="color: #000000;"> BaseController : Controller
    {
        </span><span style="color: #0000ff;">protected</span> <span style="color: #0000ff;">override</span> <span style="color: #0000ff;">void</span><span style="color: #000000;"> OnException(ExceptionContext filterContext)
        {
            </span><span style="color: #0000ff;">string</span> controllerName = (<span style="color: #0000ff;">string</span>)filterContext.RouteData.Values[<span style="color: #800000;">"</span><span style="color: #800000;">controller</span><span style="color: #800000;">"</span><span style="color: #000000;">];
            </span><span style="color: #0000ff;">string</span> actionName = (<span style="color: #0000ff;">string</span>)filterContext.RouteData.Values[<span style="color: #800000;">"</span><span style="color: #800000;">action</span><span style="color: #800000;">"</span><span style="color: #000000;">];
            HandleErrorInfo info </span>= <span style="color: #0000ff;">new</span><span style="color: #000000;"> HandleErrorInfo(filterContext.Exception, controllerName, actionName);

            DateTime dt </span>=<span style="color: #000000;"> DateTime.Now;
            </span><span style="color: #0000ff;">string</span> logPath = Server.MapPath(<span style="color: #800000;">"</span><span style="color: #800000;">~/Logs/</span><span style="color: #800000;">"</span> + dt.ToString(<span style="color: #800000;">"</span><span style="color: #800000;">yyyy-MM</span><span style="color: #800000;">"</span><span style="color: #000000;">));
            </span><span style="color: #0000ff;">if</span> (!<span style="color: #000000;">Directory.Exists(logPath))
            {
                Directory.CreateDirectory(logPath);
            }
            </span><span style="color: #0000ff;">string</span> logFilePath = <span style="color: #0000ff;">string</span>.Format(<span style="color: #800000;">"</span><span style="color: #800000;">{0}/{1}.txt</span><span style="color: #800000;">"</span>, logPath, dt.ToString(<span style="color: #800000;">"</span><span style="color: #800000;">yyyy-MM-dd</span><span style="color: #800000;">"</span><span style="color: #000000;">));
            StreamWriter writer </span>= <span style="color: #0000ff;">null</span><span style="color: #000000;">;
            </span><span style="color: #0000ff;">try</span><span style="color: #000000;">
            {
                writer </span>= <span style="color: #0000ff;">new</span> StreamWriter(logFilePath, <span style="color: #0000ff;">true</span><span style="color: #000000;">, Encoding.UTF8);
                writer.WriteLine(</span><span style="color: #800000;">"</span><span style="color: #800000;">------------------------------------------------------------------------------</span><span style="color: #800000;">"</span><span style="color: #000000;">);
                writer.WriteLine(</span><span style="color: #800000;">"</span><span style="color: #800000;">出错时间：</span><span style="color: #800000;">"</span> + DateTime.Now.ToString(<span style="color: #800000;">"</span><span style="color: #800000;">yyyy-MM-dd HH:mm:ss</span><span style="color: #800000;">"</span><span style="color: #000000;">));
                writer.WriteLine(</span><span style="color: #800000;">"</span><span style="color: #800000;">错误信息：</span><span style="color: #800000;">"</span> +<span style="color: #000000;"> filterContext.Exception.Message);
                writer.WriteLine(</span><span style="color: #800000;">"</span><span style="color: #800000;">Controller：</span><span style="color: #800000;">"</span> +<span style="color: #000000;"> filterContext.Controller);
                writer.WriteLine(</span><span style="color: #800000;">"</span><span style="color: #800000;">错误源：</span><span style="color: #800000;">"</span> +<span style="color: #000000;"> filterContext.Exception.Source);
                writer.WriteLine(</span><span style="color: #800000;">"</span><span style="color: #800000;">堆栈信息：</span><span style="color: #800000;">"</span> +<span style="color: #000000;"> filterContext.Exception.StackTrace);
                writer.WriteLine(</span><span style="color: #800000;">"</span><span style="color: #800000;">------------------------------------------------------------------------------</span><span style="color: #800000;">"</span><span style="color: #000000;">);
            }
            </span><span style="color: #0000ff;">catch</span><span style="color: #000000;">
            {
            }
            </span><span style="color: #0000ff;">finally</span><span style="color: #000000;">
            {
                </span><span style="color: #0000ff;">if</span> (writer != <span style="color: #0000ff;">null</span><span style="color: #000000;">)
                {
                    writer.Close();
                }
            }
            </span><span style="color: #0000ff;">base</span><span style="color: #000000;">.OnException(filterContext);
            filterContext.Result </span>= <span style="color: #0000ff;">new</span><span style="color: #000000;"> ViewResult() 
            {
                ViewName </span>= <span style="color: #800000;">"</span><span style="color: #800000;">/Views/Shared/Error.cshtml</span><span style="color: #800000;">"</span><span style="color: #000000;">,
                ViewData </span>= <span style="color: #0000ff;">new</span> ViewDataDictionary&lt;HandleErrorInfo&gt;<span style="color: #000000;">(info)
            };
        }

        </span><span style="color: #0000ff;">public</span><span style="color: #000000;"> ActionResult Error()
        {
            </span><span style="color: #0000ff;">return</span><span style="color: #000000;"> View();
        }
    }</span></pre>
</div>
<span class="cnblogs_code_collapse">基础控制器实现</span></div>
<div class="cnblogs_code" onclick="cnblogs_code_show('17edb7ac-be40-488e-bd80-8e6676509883')"><img id="code_img_closed_17edb7ac-be40-488e-bd80-8e6676509883" class="code_img_closed" src="http://images.cnblogs.com/OutliningIndicators/ContractedBlock.gif" alt="" /><img id="code_img_opened_17edb7ac-be40-488e-bd80-8e6676509883" class="code_img_opened" style="display: none;" onclick="cnblogs_code_hide('17edb7ac-be40-488e-bd80-8e6676509883',event)" src="http://images.cnblogs.com/OutliningIndicators/ExpandedBlockStart.gif" alt="" />
<div id="cnblogs_code_open_17edb7ac-be40-488e-bd80-8e6676509883" class="cnblogs_code_hide">
<pre> <span style="color: #0000ff;">public</span> <span style="color: #0000ff;">class</span><span style="color: #000000;"> HomeController : BaseController
    {
        </span><span style="color: #008000;">//</span>
        <span style="color: #008000;">//</span><span style="color: #008000;"> GET: /Home/</span>

        <span style="color: #0000ff;">public</span><span style="color: #000000;"> ActionResult Index()
        {
            </span><span style="color: #0000ff;">var</span> x=<span style="color: #800080;">1</span><span style="color: #000000;">;
            </span><span style="color: #0000ff;">string</span> y=<span style="color: #800000;">"</span><span style="color: #800000;">x</span><span style="color: #800000;">"</span><span style="color: #000000;">;
            ViewBag.t </span>= x /<span style="color: #000000;"> Convert.ToInt32(y);
            </span><span style="color: #0000ff;">return</span><span style="color: #000000;"> View();
        }
    }</span></pre>
</div>
<span class="cnblogs_code_collapse">控制器继承</span></div>
<p>&nbsp;备注：异常日志记录类</p>
<div class="cnblogs_code" onclick="cnblogs_code_show('119e6b2a-ee29-4fa6-8a74-f517771167b9')"><img id="code_img_closed_119e6b2a-ee29-4fa6-8a74-f517771167b9" class="code_img_closed" src="http://images.cnblogs.com/OutliningIndicators/ContractedBlock.gif" alt="" /><img id="code_img_opened_119e6b2a-ee29-4fa6-8a74-f517771167b9" class="code_img_opened" style="display: none;" onclick="cnblogs_code_hide('119e6b2a-ee29-4fa6-8a74-f517771167b9',event)" src="http://images.cnblogs.com/OutliningIndicators/ExpandedBlockStart.gif" alt="" />
<div id="cnblogs_code_open_119e6b2a-ee29-4fa6-8a74-f517771167b9" class="cnblogs_code_hide">
<pre><span style="color: #0000ff;">public</span> <span style="color: #0000ff;">class</span><span style="color: #000000;"> Log
    {
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;summary&gt;</span>
        <span style="color: #808080;">///</span><span style="color: #008000;"> 异常信息记录
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;/summary&gt;</span>
        <span style="color: #808080;">///</span> <span style="color: #808080;">&lt;param name="filterContext"&gt;&lt;/param&gt;</span>
        <span style="color: #0000ff;">public</span> <span style="color: #0000ff;">void</span><span style="color: #000000;"> Write(ExceptionContext filterContext)
        {
            DateTime dt </span>=<span style="color: #000000;"> DateTime.Now;
            </span><span style="color: #0000ff;">string</span> logPath = HttpContext.Current.Server.MapPath(<span style="color: #800000;">"</span><span style="color: #800000;">~/Logs/</span><span style="color: #800000;">"</span> + dt.ToString(<span style="color: #800000;">"</span><span style="color: #800000;">yyyy-MM</span><span style="color: #800000;">"</span><span style="color: #000000;">));
            </span><span style="color: #0000ff;">if</span> (!<span style="color: #000000;">Directory.Exists(logPath))
            {
                Directory.CreateDirectory(logPath);
            }
            </span><span style="color: #0000ff;">string</span> logFilePath = <span style="color: #0000ff;">string</span>.Format(<span style="color: #800000;">"</span><span style="color: #800000;">{0}/{1}.txt</span><span style="color: #800000;">"</span>, logPath, dt.ToString(<span style="color: #800000;">"</span><span style="color: #800000;">yyyy-MM-dd</span><span style="color: #800000;">"</span><span style="color: #000000;">));
            StreamWriter writer </span>= <span style="color: #0000ff;">null</span><span style="color: #000000;">;
            </span><span style="color: #0000ff;">try</span><span style="color: #000000;">
            {
                writer </span>= <span style="color: #0000ff;">new</span> StreamWriter(logFilePath, <span style="color: #0000ff;">true</span><span style="color: #000000;">, Encoding.UTF8);
                writer.WriteLine(</span><span style="color: #800000;">"</span><span style="color: #800000;">------------------------------------------------------------------------------</span><span style="color: #800000;">"</span><span style="color: #000000;">);
                writer.WriteLine(</span><span style="color: #800000;">"</span><span style="color: #800000;">出错时间：</span><span style="color: #800000;">"</span> + DateTime.Now.ToString(<span style="color: #800000;">"</span><span style="color: #800000;">yyyy-MM-dd HH:mm:ss</span><span style="color: #800000;">"</span><span style="color: #000000;">));
                writer.WriteLine(</span><span style="color: #800000;">"</span><span style="color: #800000;">错误信息：</span><span style="color: #800000;">"</span> +<span style="color: #000000;"> filterContext.Exception.Message);
                writer.WriteLine(</span><span style="color: #800000;">"</span><span style="color: #800000;">Controller：</span><span style="color: #800000;">"</span> +<span style="color: #000000;"> filterContext.Controller);
                writer.WriteLine(</span><span style="color: #800000;">"</span><span style="color: #800000;">错误源：</span><span style="color: #800000;">"</span> +<span style="color: #000000;"> filterContext.Exception.Source);
                writer.WriteLine(</span><span style="color: #800000;">"</span><span style="color: #800000;">堆栈信息：</span><span style="color: #800000;">"</span> +<span style="color: #000000;"> filterContext.Exception.StackTrace);
                writer.WriteLine(</span><span style="color: #800000;">"</span><span style="color: #800000;">------------------------------------------------------------------------------</span><span style="color: #800000;">"</span><span style="color: #000000;">);
            }
            </span><span style="color: #0000ff;">catch</span><span style="color: #000000;">
            {
            }
            </span><span style="color: #0000ff;">finally</span><span style="color: #000000;">
            {
                </span><span style="color: #0000ff;">if</span> (writer != <span style="color: #0000ff;">null</span><span style="color: #000000;">)
                {
                    writer.Close();
                }
            }
        }
    }</span></pre>
</div>
<span class="cnblogs_code_collapse">Log</span></div>
<p>4.还可以通过Global.apsx添加Application_Error方法来实现，但实验过程中不怎么喜欢这种方案，故省略。</p>