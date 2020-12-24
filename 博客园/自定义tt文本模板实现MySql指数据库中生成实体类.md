<p>1.在项目中依次点击&ldquo;添加&rdquo;/&ldquo;新建项&rdquo;，选择&ldquo;文本模板&rdquo;，输入名称后点击添加。</p>
<p><img style="float: left;" src="http://images2017.cnblogs.com/blog/1159701/201708/1159701-20170826220208355-1859306590.png" alt="" /></p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p><img src="http://images2017.cnblogs.com/blog/1159701/201708/1159701-20170826220321480-2086704050.png" alt="" /></p>
<p>2.在Base.tt中添加如下代码。</p>
<div class="cnblogs_code">
<pre>&lt;#@ template debug=<span style="color: #800000;">"</span><span style="color: #800000;">false</span><span style="color: #800000;">"</span> hostspecific=<span style="color: #800000;">"</span><span style="color: #800000;">false</span><span style="color: #800000;">"</span> language=<span style="color: #800000;">"</span><span style="color: #800000;">C#</span><span style="color: #800000;">"</span> #&gt;
&lt;#@ output extension=<span style="color: #800000;">"</span><span style="color: #800000;">.cs</span><span style="color: #800000;">"</span> #&gt;
&lt;#@ Assembly Name=<span style="color: #800000;">"</span><span style="color: #800000;">System.Core</span><span style="color: #800000;">"</span> #&gt;
&lt;#@ Assembly Name=<span style="color: #800000;">"</span><span style="color: #800000;">System.Windows.Forms</span><span style="color: #800000;">"</span> #&gt;
&lt;#@ Assembly Name=<span style="color: #800000;">"</span><span style="color: #800000;">MySql.Data.dll</span><span style="color: #800000;">"</span> #&gt;
&lt;#@ Assembly Name=<span style="color: #800000;">"</span><span style="color: #800000;">System.Data</span><span style="color: #800000;">"</span> #&gt;
&lt;#@ Assembly Name=<span style="color: #800000;">"</span><span style="color: #800000;">System</span><span style="color: #800000;">"</span> #&gt;
&lt;#@ import <span style="color: #0000ff;">namespace</span>=<span style="color: #800000;">"</span><span style="color: #800000;">System</span><span style="color: #800000;">"</span> #&gt;
&lt;#@ import <span style="color: #0000ff;">namespace</span>=<span style="color: #800000;">"</span><span style="color: #800000;">System.IO</span><span style="color: #800000;">"</span> #&gt;
&lt;#@ import <span style="color: #0000ff;">namespace</span>=<span style="color: #800000;">"</span><span style="color: #800000;">System.Diagnostics</span><span style="color: #800000;">"</span> #&gt;
&lt;#@ import <span style="color: #0000ff;">namespace</span>=<span style="color: #800000;">"</span><span style="color: #800000;">System.Linq</span><span style="color: #800000;">"</span> #&gt;
&lt;#@ import <span style="color: #0000ff;">namespace</span>=<span style="color: #800000;">"</span><span style="color: #800000;">System.Collections</span><span style="color: #800000;">"</span> #&gt;
&lt;#@ import <span style="color: #0000ff;">namespace</span>=<span style="color: #800000;">"</span><span style="color: #800000;">System.Collections.Generic</span><span style="color: #800000;">"</span> #&gt; 
&lt;#@ import <span style="color: #0000ff;">namespace</span>=<span style="color: #800000;">"</span><span style="color: #800000;">MySql.Data</span><span style="color: #800000;">"</span> #&gt;
&lt;#@ import <span style="color: #0000ff;">namespace</span>=<span style="color: #800000;">"</span><span style="color: #800000;">MySql.Data.MySqlClient</span><span style="color: #800000;">"</span> #&gt;

<span style="color: #008000;">//</span><span style="color: #008000;">数据库基本信息设置</span>
&lt;<span style="color: #000000;"># 
    </span><span style="color: #0000ff;">string</span> Server=<span style="color: #800000;">"</span><span style="color: #800000;">localhost</span><span style="color: #800000;">"</span><span style="color: #000000;">;
    </span><span style="color: #0000ff;">string</span> UId=<span style="color: #800000;">"</span><span style="color: #800000;">root</span><span style="color: #800000;">"</span><span style="color: #000000;">;
    </span><span style="color: #0000ff;">string</span> PWd=<span style="color: #800000;">"</span><span style="color: #800000;">fxy19940923..</span><span style="color: #800000;">"</span><span style="color: #000000;">;
    </span><span style="color: #0000ff;">string</span> Db_Name=<span style="color: #800000;">"</span><span style="color: #800000;">cxkdb</span><span style="color: #800000;">"</span><span style="color: #000000;">;
#</span>&gt;
<span style="color: #008000;">//</span><span style="color: #008000;">获取数据库中各表基本数据</span>
&lt;<span style="color: #000000;">#
    </span><span style="color: #0000ff;">string</span> conStr=<span style="color: #800000;">"</span><span style="color: #800000;">server=</span><span style="color: #800000;">"</span>+Server+<span style="color: #800000;">"</span><span style="color: #800000;">;database=</span><span style="color: #800000;">"</span>+Db_Name+<span style="color: #800000;">"</span><span style="color: #800000;">;uid=</span><span style="color: #800000;">"</span>+UId+<span style="color: #800000;">"</span><span style="color: #800000;">;pwd=</span><span style="color: #800000;">"</span>+<span style="color: #000000;">PWd;
    MySqlConnection con</span>=<span style="color: #0000ff;">new</span><span style="color: #000000;"> MySqlConnection(conStr);
    con.Open();
    MySqlParameter mp</span>=<span style="color: #0000ff;">new</span> MySqlParameter(<span style="color: #800000;">"</span><span style="color: #800000;">@db_name</span><span style="color: #800000;">"</span><span style="color: #000000;">,Db_Name);
    </span><span style="color: #0000ff;">string</span> sqlStr=<span style="color: #800000;">"</span><span style="color: #800000;">select * from information_schema.COLUMNS where table_schema=@db_name </span><span style="color: #800000;">"</span><span style="color: #000000;">;
    MySqlCommand cmd</span>=<span style="color: #0000ff;">new</span><span style="color: #000000;"> MySqlCommand(sqlStr,con);
    cmd.Parameters.Add(mp);
    MySqlDataReader dr </span>=<span style="color: #000000;"> cmd.ExecuteReader();

    </span><span style="color: #0000ff;">var</span> TableList=<span style="color: #0000ff;">new</span> List&lt;<span style="color: #0000ff;">string</span>&gt;<span style="color: #000000;">();
    </span><span style="color: #0000ff;">var</span> DetailList=<span style="color: #0000ff;">new</span> List&lt;Tuple&lt;<span style="color: #0000ff;">string</span>,<span style="color: #0000ff;">string</span>,<span style="color: #0000ff;">string</span>&gt;&gt;<span style="color: #000000;">();
    </span><span style="color: #0000ff;">var</span> TableName=<span style="color: #800000;">""</span><span style="color: #000000;">;
    </span><span style="color: #0000ff;">while</span><span style="color: #000000;">(dr.Read())
    {
        </span><span style="color: #0000ff;">var</span> Table=dr[<span style="color: #800000;">"</span><span style="color: #800000;">TABLE_NAME</span><span style="color: #800000;">"</span><span style="color: #000000;">].ToString();
        </span><span style="color: #0000ff;">if</span>(TableName==<span style="color: #000000;">Table)
        {
            </span><span style="color: #0000ff;">var</span> Column=dr[<span style="color: #800000;">"</span><span style="color: #800000;">COLUMN_NAME</span><span style="color: #800000;">"</span><span style="color: #000000;">].ToString();
            </span><span style="color: #0000ff;">var</span> DataTypeOld=dr[<span style="color: #800000;">"</span><span style="color: #800000;">DATA_TYPE</span><span style="color: #800000;">"</span><span style="color: #000000;">].ToString();
            </span><span style="color: #0000ff;">var</span> DataType=DataTypeOld==<span style="color: #800000;">"</span><span style="color: #800000;">varchar</span><span style="color: #800000;">"</span>?<span style="color: #800000;">"</span><span style="color: #800000;">string</span><span style="color: #800000;">"</span><span style="color: #000000;">:
                (DataTypeOld</span>==<span style="color: #800000;">"</span><span style="color: #800000;">int</span><span style="color: #800000;">"</span>?<span style="color: #800000;">"</span><span style="color: #800000;">int</span><span style="color: #800000;">"</span><span style="color: #000000;">:
                (DataTypeOld</span>==<span style="color: #800000;">"</span><span style="color: #800000;">datetime</span><span style="color: #800000;">"</span>?<span style="color: #800000;">"</span><span style="color: #800000;">DateTime</span><span style="color: #800000;">"</span><span style="color: #000000;">:
                (DataTypeOld</span>==<span style="color: #800000;">"</span><span style="color: #800000;">decimal</span><span style="color: #800000;">"</span>?<span style="color: #800000;">"</span><span style="color: #800000;">decimal</span><span style="color: #800000;">"</span><span style="color: #000000;">:
                (DataTypeOld</span>==<span style="color: #800000;">"</span><span style="color: #800000;">char</span><span style="color: #800000;">"</span>?<span style="color: #800000;">"</span><span style="color: #800000;">string</span><span style="color: #800000;">"</span><span style="color: #000000;">:
                (DataTypeOld</span>==<span style="color: #800000;">"</span><span style="color: #800000;">bit</span><span style="color: #800000;">"</span>?<span style="color: #800000;">"</span><span style="color: #800000;">Boolean</span><span style="color: #800000;">"</span>:<span style="color: #800000;">"</span><span style="color: #800000;">string</span><span style="color: #800000;">"</span><span style="color: #000000;">)))));
            DetailList.Add(Tuple.Create(Table,DataType,Column));
        }
        </span><span style="color: #0000ff;">else</span><span style="color: #000000;">
        {
            TableList.Add(Table);
        }
        TableName</span>=<span style="color: #000000;">Table;
    }
#</span>&gt;
<span style="color: #0000ff;">using</span><span style="color: #000000;"> System;
</span><span style="color: #0000ff;">using</span><span style="color: #000000;"> System.Collections.Generic;
</span><span style="color: #0000ff;">using</span><span style="color: #000000;"> System.Linq;
</span><span style="color: #0000ff;">using</span><span style="color: #000000;"> System.Text;
</span><span style="color: #0000ff;">using</span> System.Data;</pre>
</div>
<p>3.利用1中的方法新建DataBaseEntity.tt文本模板，并添加如下代码。</p>
<div class="cnblogs_code">
<pre>&lt;#@ template debug=<span style="color: #800000;">"</span><span style="color: #800000;">false</span><span style="color: #800000;">"</span> hostspecific=<span style="color: #800000;">"</span><span style="color: #800000;">false</span><span style="color: #800000;">"</span> language=<span style="color: #800000;">"</span><span style="color: #800000;">C#</span><span style="color: #800000;">"</span> #&gt;
&lt;#@ output extension=<span style="color: #800000;">"</span><span style="color: #800000;">.cs</span><span style="color: #800000;">"</span> #&gt;
&lt;#@  include file=<span style="color: #800000;">"</span><span style="color: #800000;">Base.tt</span><span style="color: #800000;">"</span> #&gt;

<span style="color: #0000ff;">namespace</span><span style="color: #000000;"> DataBaseEntity
{
</span>&lt;# <span style="color: #0000ff;">foreach</span>(<span style="color: #0000ff;">var</span> item <span style="color: #0000ff;">in</span> TableList) { #&gt;
    <span style="color: #0000ff;">public</span> <span style="color: #0000ff;">class</span> &lt;#= item #&gt;<span style="color: #000000;">
    {</span>&lt;# <span style="color: #0000ff;">foreach</span>(<span style="color: #0000ff;">var</span> detail <span style="color: #0000ff;">in</span> DetailList) {<span style="color: #0000ff;">if</span>( item == detail.Item1 ) { #&gt; 
        <span style="color: #0000ff;">public</span> &lt;#=detail.Item2#&gt; &lt;#=detail.Item3#&gt; { <span style="color: #0000ff;">get</span>; <span style="color: #0000ff;">set</span><span style="color: #000000;">; } 
</span>&lt;#}}#&gt;<span style="color: #000000;">    }
</span>&lt;# } #&gt;}</pre>
</div>
<p>4.将数据库的基本信息填写在Base.tt文件中，即&ldquo;数据库基本设置&rdquo;部分，然后依次保存Base.tt、DataBaseEntity.tt,在DataBase.tt目录下会生成对应的实体类。</p>
<p><img src="http://images2017.cnblogs.com/blog/1159701/201708/1159701-20170826221107714-900695281.png" alt="" /></p>
<p>&nbsp;</p>