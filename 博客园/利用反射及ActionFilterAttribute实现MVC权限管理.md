<p>1.利用反射获取当前程序集下的所有控制器和方法，拼接后写入到数据库。</p>
<div class="cnblogs_code" onclick="cnblogs_code_show('55925988-a371-423d-8b7d-6ad0e95f8dc6')"><img id="code_img_closed_55925988-a371-423d-8b7d-6ad0e95f8dc6" class="code_img_closed" src="http://images.cnblogs.com/OutliningIndicators/ContractedBlock.gif" alt="" /><img id="code_img_opened_55925988-a371-423d-8b7d-6ad0e95f8dc6" class="code_img_opened" style="display: none;" onclick="cnblogs_code_hide('55925988-a371-423d-8b7d-6ad0e95f8dc6',event)" src="http://images.cnblogs.com/OutliningIndicators/ExpandedBlockStart.gif" alt="" />
<div id="cnblogs_code_open_55925988-a371-423d-8b7d-6ad0e95f8dc6" class="cnblogs_code_hide">
<pre> <span style="color: #0000ff;">public</span> <span style="color: #0000ff;">void</span><span style="color: #000000;"> GetRightInfo()
        {
            </span><span style="color: #0000ff;">var</span> ControllerIDMax = db.rights_info.Select(p =&gt; p.RightsID).Max() + <span style="color: #800080;">1</span><span style="color: #000000;">;
            </span><span style="color: #0000ff;">var</span> controllerTypes = Assembly.GetExecutingAssembly().GetTypes().Where(p =&gt; <span style="color: #0000ff;">typeof</span><span style="color: #000000;">(IController).IsAssignableFrom(p));

            </span><span style="color: #0000ff;">foreach</span> (<span style="color: #0000ff;">var</span> item <span style="color: #0000ff;">in</span><span style="color: #000000;"> controllerTypes)
            {
                </span><span style="color: #0000ff;">var</span> actionMethods = item.GetMethods().Where(q =&gt; q.ReturnType.Name == <span style="color: #800000;">"</span><span style="color: #800000;">ActionResult</span><span style="color: #800000;">"</span><span style="color: #000000;">);
                </span><span style="color: #0000ff;">foreach</span> (<span style="color: #0000ff;">var</span> action <span style="color: #0000ff;">in</span><span style="color: #000000;"> actionMethods)
                {
                    </span><span style="color: #0000ff;">var</span> rightsName = item.Name.Replace(<span style="color: #800000;">"</span><span style="color: #800000;">Controller</span><span style="color: #800000;">"</span>, <span style="color: #800000;">""</span>).ToLower() + <span style="color: #800000;">"</span><span style="color: #800000;">/</span><span style="color: #800000;">"</span> +<span style="color: #000000;"> action.Name.ToLower();
                    </span><span style="color: #0000ff;">var</span> ControllerInfo = <span style="color: #0000ff;">new</span><span style="color: #000000;"> rights_info()
                    {
                        RightsID </span>=<span style="color: #000000;"> ControllerIDMax,
                        RightsName </span>=<span style="color: #000000;">rightsName
                    };
                    </span><span style="color: #0000ff;">if</span> (db.rights_info.Where(p =&gt; p.RightsName == rightsName).Count() == <span style="color: #800080;">0</span><span style="color: #000000;">)
                    {
                        db.rights_info.AddObject(ControllerInfo);
                        ControllerIDMax</span>++<span style="color: #000000;">;
                    }
                }
            }
            db.SaveChanges();
        }</span></pre>
</div>
<span class="cnblogs_code_collapse">Get Url</span></div>
<p>2.重写ActionFilterAttribute的OnActionExecuting方法实现自定义action权限访问。</p>
<div class="cnblogs_code" onclick="cnblogs_code_show('99d9f8bc-1cfa-4f3c-bc88-9b3166d2f03a')"><img id="code_img_closed_99d9f8bc-1cfa-4f3c-bc88-9b3166d2f03a" class="code_img_closed" src="http://images.cnblogs.com/OutliningIndicators/ContractedBlock.gif" alt="" /><img id="code_img_opened_99d9f8bc-1cfa-4f3c-bc88-9b3166d2f03a" class="code_img_opened" style="display: none;" onclick="cnblogs_code_hide('99d9f8bc-1cfa-4f3c-bc88-9b3166d2f03a',event)" src="http://images.cnblogs.com/OutliningIndicators/ExpandedBlockStart.gif" alt="" />
<div id="cnblogs_code_open_99d9f8bc-1cfa-4f3c-bc88-9b3166d2f03a" class="cnblogs_code_hide">
<pre>  <span style="color: #0000ff;">public</span> <span style="color: #0000ff;">override</span> <span style="color: #0000ff;">void</span><span style="color: #000000;"> OnActionExecuting(ActionExecutingContext filterContext)
        {
            </span><span style="color: #008000;">//</span><span style="color: #008000;">url of visit</span>
            <span style="color: #0000ff;">var</span> controllerName = filterContext.RouteData.Values[<span style="color: #800000;">"</span><span style="color: #800000;">controller</span><span style="color: #800000;">"</span><span style="color: #000000;">].ToString().ToLower();
            </span><span style="color: #0000ff;">var</span> actionName = filterContext.RouteData.Values[<span style="color: #800000;">"</span><span style="color: #800000;">action</span><span style="color: #800000;">"</span><span style="color: #000000;">].ToString().ToLower();
            </span><span style="color: #0000ff;">var</span> url = controllerName + <span style="color: #800000;">"</span><span style="color: #800000;">/</span><span style="color: #800000;">"</span> +<span style="color: #000000;"> actionName;

            </span><span style="color: #008000;">//</span><span style="color: #008000;">get rights of user</span>
            <span style="color: #0000ff;">var</span> userInfo = HttpContext.Current.Session[<span style="color: #800000;">"</span><span style="color: #800000;">UserId</span><span style="color: #800000;">"</span>] == <span style="color: #0000ff;">null</span> ? <span style="color: #800000;">""</span> : HttpContext.Current.Session[<span style="color: #800000;">"</span><span style="color: #800000;">UserId</span><span style="color: #800000;">"</span><span style="color: #000000;">].ToString();
            </span><span style="color: #0000ff;">var</span> right = db.cus_cusmanagersinfo.Where(p =&gt; p.cus_Id == userInfo).Select(p =&gt; p.cus_Rights).First().Split(<span style="color: #800000;">'</span><span style="color: #800000;">,</span><span style="color: #800000;">'</span><span style="color: #000000;">);

            </span><span style="color: #008000;">//</span><span style="color: #008000;">check</span>
            <span style="color: #0000ff;">long</span> t = db.rights_info.Where(p =&gt; p.RightsName == url).Select(p =&gt;<span style="color: #000000;"> p.RightsID).First();
            </span><span style="color: #0000ff;">var</span> check =<span style="color: #000000;"> right.Contains(t.ToString());
            </span><span style="color: #0000ff;">if</span> (!<span style="color: #000000;">check)
            {
                </span><span style="color: #008000;">//</span><span style="color: #008000;">Redirection</span>
                filterContext.Result = <span style="color: #0000ff;">new</span> RedirectResult(<span style="color: #800000;">"</span><span style="color: #800000;">/home/index</span><span style="color: #800000;">"</span><span style="color: #000000;">);
            }
        }</span></pre>
</div>
<span class="cnblogs_code_collapse">重写OnActionExecuting</span></div>