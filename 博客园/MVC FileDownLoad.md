<div class="cnblogs_code">
<pre>        <span style="color: #0000ff;">public</span><span style="color: #000000;"> ActionResult MatDownload()
        {    
            </span><span style="color: #0000ff;">string</span> ShopId = Session[<span style="color: #800000;">"</span><span style="color: #800000;">ShopId</span><span style="color: #800000;">"</span><span style="color: #000000;">].ToString();
            </span><span style="color: #0000ff;">var</span> selfStock = <span style="color: #0000ff;">from</span> c <span style="color: #0000ff;">in</span><span style="color: #000000;"> db.stock_infolist
                            join d </span><span style="color: #0000ff;">in</span><span style="color: #000000;"> db.bom on c.Stock_Sn equals d.Bom_Sn
                            </span><span style="color: #0000ff;">where</span> c.Stock_ShopID == ShopId &amp;&amp; c.Stock_Code.Length &lt;= <span style="color: #800080;">19</span>
                            <span style="color: #0000ff;">select</span> <span style="color: #0000ff;">new</span><span style="color: #000000;"> StockOut
                            {
                                Stock_Sn </span>=<span style="color: #000000;"> c.Stock_Sn,
                                Bom_Type </span>=<span style="color: #000000;"> d.Bom_Type,
                                Bom_Category </span>=<span style="color: #000000;"> d.Bom_Category,
                                Bom_Name </span>=<span style="color: #000000;"> d.Bom_Name,
                                Bom_Brand </span>=<span style="color: #000000;"> d.Bom_Brand,
                                Bom_Model </span>=<span style="color: #000000;"> d.Bom_Model,
                                Stock_Amount </span>= c.Stock_Amount ?? <span style="color: #800080;">0</span><span style="color: #000000;">,
                                Stock_Univalent </span>= c.Stock_Univalent ?? <span style="color: #800080;">0</span><span style="color: #000000;">,
                                SuggestedPrice </span>= c.SuggestedPrice ?? <span style="color: #800080;">0</span><span style="color: #000000;">
                            };
            </span><span style="color: #0000ff;">var</span> enList =<span style="color: #000000;"> selfStock.ToList();
            </span><span style="color: #0000ff;">string</span> url = <span style="color: #800000;">"</span><span style="color: #800000;">~/files/Out/Mat</span><span style="color: #800000;">"</span><span style="color: #000000;">;
            </span><span style="color: #0000ff;">string</span> fileName = Server.MapPath(url + <span style="color: #800000;">"</span><span style="color: #800000;">.xlsx</span><span style="color: #800000;">"</span>);<span style="color: #008000;">//</span><span style="color: #008000;">保存文件名</span>
            <span style="color: #0000ff;">string</span> fileDName = Server.MapPath(url + DateTime.Now.ToString(<span style="color: #800000;">"</span><span style="color: #800000;">yyyyMMddhhmmss</span><span style="color: #800000;">"</span>) + <span style="color: #800000;">"</span><span style="color: #800000;">.xlsx</span><span style="color: #800000;">"</span>);<span style="color: #008000;">//</span><span style="color: #008000;">下载文件名</span>
            ExcelHandle eh = <span style="color: #0000ff;">new</span><span style="color: #000000;"> ExcelHandle();
            eh.IListToExcel(enList, fileName, </span><span style="color: #800000;">"</span><span style="color: #800000;">已有物料</span><span style="color: #800000;">"</span>, <span style="color: #800000;">"</span><span style="color: #800000;">店铺</span><span style="color: #800000;">"</span> + ShopId + <span style="color: #800000;">"</span><span style="color: #800000;">拥有的物料信息</span><span style="color: #800000;">"</span><span style="color: #000000;">);//实现转换保存
            </span><span style="color: #0000ff;">return</span> File(fileName, <span style="color: #800000;">"</span><span style="color: #800000;">application/ms-excel</span><span style="color: #800000;">"</span>, Url.Encode(Path.GetFileName(fileDName)));<span style="color: #008000;">//发送文件到客户端
</span><span style="color: #000000;">            }
        }
    }</span></pre>
</div>
<p><span style="color: #3366ff;"><strong>1.LINQ从数据库获取数据，格式为List。</strong></span></p>
<p><span style="color: #3366ff;"><strong>2.指定服务器端保存文件名，及客户端下载文件名。</strong></span></p>
<p><span style="color: #3366ff;"><strong>3.调用ExcelHandle中的IListToExcel方法，完成转换及保存。</strong></span></p>
<p><span style="color: #3366ff;"><strong>4.发送文件到客户端，View中直接调用MatDownload()方法即可实现下载。</strong></span></p>