<div class="cnblogs_code">
<pre><span style="color: #0000ff;">using</span><span style="color: #000000;"> System.Text;
</span><span style="color: #0000ff;">using</span><span style="color: #000000;"> MySql.Data.MySqlClient;
</span><span style="color: #0000ff;">using</span><span style="color: #000000;"> System.Data;
</span><span style="color: #0000ff;">using</span><span style="color: #000000;"> Dapper;
</span><span style="color: #0000ff;">using</span><span style="color: #000000;"> System.Reflection;

</span><span style="color: #0000ff;">namespace</span><span style="color: #000000;"> DbHelper
{
    </span><span style="color: #0000ff;">public</span> <span style="color: #0000ff;">class</span><span style="color: #000000;"> MySqlHelper
    {
        </span><span style="color: #0000ff;">private</span> <span style="color: #0000ff;">string</span> connectionStr = <span style="color: #800000;">"</span><span style="color: #800000;">server=localhost;database=fxy;User=root;password=cxk</span><span style="color: #800000;">"</span><span style="color: #000000;">;
        </span><span style="color: #008000;">//</span><span style="color: #008000;">public object connection = GetConnection(connectionStr);</span>


        <span style="color: #808080;">///</span> <span style="color: #808080;">&lt;summary&gt;</span>
        <span style="color: #808080;">///</span><span style="color: #008000;"> Dapper查询（包含存储过程及sql语句查询）
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;/summary&gt;</span>
        <span style="color: #808080;">///</span> <span style="color: #808080;">&lt;typeparam name="T"&gt;</span><span style="color: #008000;">实体类型</span><span style="color: #808080;">&lt;/typeparam&gt;</span>
        <span style="color: #808080;">///</span> <span style="color: #808080;">&lt;param name="sql"&gt;</span><span style="color: #008000;">存储过程名称或者sql语句</span><span style="color: #808080;">&lt;/param&gt;</span>
        <span style="color: #808080;">///</span> <span style="color: #808080;">&lt;param name="param"&gt;</span><span style="color: #008000;">参数化处理</span><span style="color: #808080;">&lt;/param&gt;</span>
        <span style="color: #808080;">///</span> <span style="color: #808080;">&lt;param name="isStoredProcedure"&gt;</span><span style="color: #008000;">是否存储过程查询</span><span style="color: #808080;">&lt;/param&gt;</span>
        <span style="color: #808080;">///</span> <span style="color: #808080;">&lt;returns&gt;&lt;/returns&gt;</span>
        <span style="color: #0000ff;">public</span> List&lt;T&gt; DapperQuery&lt;T&gt;(<span style="color: #0000ff;">string</span> sql , <span style="color: #0000ff;">object</span> param , <span style="color: #0000ff;">bool</span>? isStoredProcedure = <span style="color: #0000ff;">false</span>) <span style="color: #0000ff;">where</span> T : <span style="color: #0000ff;">new</span><span style="color: #000000;">()
        {
            </span><span style="color: #0000ff;">using</span>(IDbConnection con = <span style="color: #0000ff;">new</span><span style="color: #000000;"> MySqlConnection(connectionStr))
            {
                CommandType cmdType </span>= (isStoredProcedure ?? <span style="color: #0000ff;">true</span>) ?<span style="color: #000000;"> CommandType.StoredProcedure : CommandType.Text;
                </span><span style="color: #0000ff;">try</span><span style="color: #000000;">
                {
                    List</span>&lt;T&gt; queryList = con.Query&lt;T&gt;(sql , param , <span style="color: #0000ff;">null</span>, <span style="color: #0000ff;">true</span> , <span style="color: #0000ff;">null</span><span style="color: #000000;"> , cmdType).ToList();
                    </span><span style="color: #0000ff;">return</span><span style="color: #000000;"> queryList;
                }
                </span><span style="color: #0000ff;">catch</span><span style="color: #000000;">(Exception e)
                {
                    </span><span style="color: #0000ff;">throw</span><span style="color: #000000;">;
                }
            }
        }

        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;summary&gt;</span>
        <span style="color: #808080;">///</span><span style="color: #008000;"> TSQL查询
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;/summary&gt;</span>
        <span style="color: #808080;">///</span> <span style="color: #808080;">&lt;typeparam name="T"&gt;&lt;/typeparam&gt;</span>
        <span style="color: #808080;">///</span> <span style="color: #808080;">&lt;param name="sql"&gt;&lt;/param&gt;</span>
        <span style="color: #808080;">///</span> <span style="color: #808080;">&lt;param name="param"&gt;&lt;/param&gt;</span>
        <span style="color: #808080;">///</span> <span style="color: #808080;">&lt;param name="isStoredProcedure"&gt;&lt;/param&gt;</span>
        <span style="color: #808080;">///</span> <span style="color: #808080;">&lt;returns&gt;&lt;/returns&gt;</span>
        <span style="color: #0000ff;">public</span> List&lt;T&gt; TSqlQuery&lt;T&gt;(<span style="color: #0000ff;">string</span> sql,MySqlParameter[] param,<span style="color: #0000ff;">bool</span>? isStoredProcedure = <span style="color: #0000ff;">false</span>) <span style="color: #0000ff;">where</span> T:<span style="color: #0000ff;">new</span><span style="color: #000000;">()
        {
            </span><span style="color: #0000ff;">using</span>(MySqlConnection con = <span style="color: #0000ff;">new</span><span style="color: #000000;"> MySqlConnection(connectionStr))
            {
                con.Open();
                CommandType cmdType </span>= (isStoredProcedure ?? <span style="color: #0000ff;">true</span>) ?<span style="color: #000000;"> CommandType.StoredProcedure : CommandType.Text;
                MySqlCommand command </span>= <span style="color: #0000ff;">new</span><span style="color: #000000;"> MySqlCommand(sql , con );
                command.CommandType </span>=<span style="color: #000000;"> cmdType;
                </span><span style="color: #0000ff;">if</span>(param != <span style="color: #0000ff;">null</span><span style="color: #000000;"> )
                {
                    command.Parameters.AddRange(param);
                }
                </span><span style="color: #0000ff;">try</span><span style="color: #000000;">
                {
                    MySqlDataReader reader </span>=<span style="color: #000000;"> command.ExecuteReader();
                    List</span>&lt;T&gt; list = DataReaderToList&lt;T&gt;<span style="color: #000000;">(reader);
                    </span><span style="color: #0000ff;">return</span><span style="color: #000000;"> list;
                }
                </span><span style="color: #0000ff;">catch</span><span style="color: #000000;">(Exception e)
                {
                    </span><span style="color: #0000ff;">throw</span><span style="color: #000000;">;
                }
                </span><span style="color: #0000ff;">finally</span><span style="color: #000000;">
                {
                    con.Close();
                }
            }
        }

        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;summary&gt;</span>
        <span style="color: #808080;">///</span><span style="color: #008000;"> Dapper增删改（包含存储过程及sql语句查询）
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;/summary&gt;</span>
        <span style="color: #808080;">///</span> <span style="color: #808080;">&lt;param name="sql"&gt;</span><span style="color: #008000;">存储过程名称或者sql语句</span><span style="color: #808080;">&lt;/param&gt;</span>
        <span style="color: #808080;">///</span> <span style="color: #808080;">&lt;param name="param"&gt;</span><span style="color: #008000;">参数化处理</span><span style="color: #808080;">&lt;/param&gt;</span>
        <span style="color: #808080;">///</span> <span style="color: #808080;">&lt;param name="isStoredProcedure"&gt;</span><span style="color: #008000;">是否存储过程查询</span><span style="color: #808080;">&lt;/param&gt;</span>
        <span style="color: #808080;">///</span> <span style="color: #808080;">&lt;returns&gt;&lt;/returns&gt;</span>
        <span style="color: #0000ff;">public</span> <span style="color: #0000ff;">bool</span> DapperExcute(<span style="color: #0000ff;">string</span> sql , <span style="color: #0000ff;">object</span> param , <span style="color: #0000ff;">bool</span>? isStoredProcedure=<span style="color: #0000ff;">false</span>,<span style="color: #0000ff;">int</span>?commandTimeout=<span style="color: #0000ff;">null</span><span style="color: #000000;">)
        {
            </span><span style="color: #0000ff;">bool</span> result = <span style="color: #0000ff;">false</span><span style="color: #000000;">;
            </span><span style="color: #0000ff;">using</span>(IDbConnection con = <span style="color: #0000ff;">new</span><span style="color: #000000;"> MySqlConnection(connectionStr))
            {
                con.Open();
                IDbTransaction tran </span>=<span style="color: #000000;"> con.BeginTransaction();
                CommandType cmdType </span>= isStoredProcedure==<span style="color: #0000ff;">true</span> ?<span style="color: #000000;"> CommandType.StoredProcedure : CommandType.Text;
                </span><span style="color: #0000ff;">try</span><span style="color: #000000;">
                {
                    </span><span style="color: #0000ff;">int</span> query =<span style="color: #000000;"> con.Execute(sql , param , tran , commandTimeout , cmdType);
                    tran.Commit();
                    result </span>= <span style="color: #0000ff;">true</span><span style="color: #000000;">;
                }
                </span><span style="color: #0000ff;">catch</span><span style="color: #000000;">(Exception e)
                {
                    tran.Rollback();
                    </span><span style="color: #0000ff;">throw</span><span style="color: #000000;">;
                }
                </span><span style="color: #0000ff;">finally</span><span style="color: #000000;">
                {
                    con.Close();
                }
                </span><span style="color: #0000ff;">return</span><span style="color: #000000;"> result;
            }
                
        }

        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;summary&gt;</span>
        <span style="color: #808080;">///</span><span style="color: #008000;"> TSQL增删改操作
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;/summary&gt;</span>
        <span style="color: #808080;">///</span> <span style="color: #808080;">&lt;param name="sql"&gt;&lt;/param&gt;</span>
        <span style="color: #808080;">///</span> <span style="color: #808080;">&lt;param name="param"&gt;&lt;/param&gt;</span>
        <span style="color: #808080;">///</span> <span style="color: #808080;">&lt;param name="isStoredProcedure"&gt;&lt;/param&gt;</span>
        <span style="color: #808080;">///</span> <span style="color: #808080;">&lt;returns&gt;&lt;/returns&gt;</span>
        <span style="color: #0000ff;">public</span> <span style="color: #0000ff;">bool</span> TSqlExcute(<span style="color: #0000ff;">string</span> sql , MySqlParameter[] param , <span style="color: #0000ff;">bool</span>? isStoredProcedure=<span style="color: #0000ff;">false</span><span style="color: #000000;">)
        {
            </span><span style="color: #0000ff;">bool</span> result = <span style="color: #0000ff;">false</span><span style="color: #000000;">;
            </span><span style="color: #0000ff;">using</span>(MySqlConnection con = <span style="color: #0000ff;">new</span><span style="color: #000000;"> MySqlConnection(connectionStr))
            {
                con.Open();
                MySqlTransaction tran </span>=<span style="color: #000000;"> con.BeginTransaction();
                CommandType cmdType </span>= isStoredProcedure==<span style="color: #0000ff;">true</span> ?<span style="color: #000000;"> CommandType.StoredProcedure : CommandType.Text;
                MySqlCommand command </span>= <span style="color: #0000ff;">new</span><span style="color: #000000;"> MySqlCommand(sql , con , tran);
                command.Parameters.AddRange(param);
                </span><span style="color: #0000ff;">try</span><span style="color: #000000;">
                {
                    </span><span style="color: #0000ff;">int</span> query =<span style="color: #000000;"> command.ExecuteNonQuery();
                    tran.Commit();
                    result </span>= <span style="color: #0000ff;">true</span><span style="color: #000000;">;
                }
                </span><span style="color: #0000ff;">catch</span><span style="color: #000000;">(Exception e)
                {
                    tran.Rollback();
                    </span><span style="color: #0000ff;">throw</span><span style="color: #000000;">;
                }
                </span><span style="color: #0000ff;">finally</span><span style="color: #000000;">
                {
                    con.Close();
                }
                </span><span style="color: #0000ff;">return</span><span style="color: #000000;"> result;
            }
        }

        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;summary&gt;</span>
        <span style="color: #808080;">///</span><span style="color: #008000;"> 批量数据写入
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;/summary&gt;</span>
        <span style="color: #808080;">///</span> <span style="color: #808080;">&lt;typeparam name="T"&gt;&lt;/typeparam&gt;</span>
        <span style="color: #808080;">///</span> <span style="color: #808080;">&lt;param name="sql"&gt;&lt;/param&gt;</span>
        <span style="color: #808080;">///</span> <span style="color: #808080;">&lt;param name="dataList"&gt;&lt;/param&gt;</span>
        <span style="color: #808080;">///</span> <span style="color: #808080;">&lt;returns&gt;&lt;/returns&gt;</span>
        <span style="color: #0000ff;">private</span> <span style="color: #0000ff;">bool</span> BulkInsert&lt;T&gt;(<span style="color: #0000ff;">string</span> sql , List&lt;T&gt; dataList) <span style="color: #0000ff;">where</span> T:<span style="color: #0000ff;">new</span><span style="color: #000000;">()
        {
            </span><span style="color: #0000ff;">bool</span> result = <span style="color: #0000ff;">false</span><span style="color: #000000;">;
            </span><span style="color: #008000;">//</span><span style="color: #008000;">获取T的公共属性</span>
            Type type = dataList[ <span style="color: #800080;">0</span><span style="color: #000000;"> ].GetType();
            PropertyInfo[] param </span>=<span style="color: #000000;"> type.GetProperties();
            List</span>&lt;<span style="color: #0000ff;">string</span>&gt; properotyList = param.Select(p =&gt;<span style="color: #000000;"> p.Name).ToList();
            </span><span style="color: #0000ff;">using</span>(MySqlConnection con= <span style="color: #0000ff;">new</span><span style="color: #000000;"> MySqlConnection(connectionStr))
            {
                con.Open();
                StringBuilder sb </span>= <span style="color: #0000ff;">new</span><span style="color: #000000;"> StringBuilder();
                sb.Append(sql);
                sb.Append(</span><span style="color: #800000;">"</span><span style="color: #800000;"> VALUES</span><span style="color: #800000;">"</span><span style="color: #000000;">);
                </span><span style="color: #0000ff;">int</span> i = <span style="color: #800080;">0</span><span style="color: #000000;">;
                </span><span style="color: #0000ff;">foreach</span>(<span style="color: #0000ff;">var</span> item <span style="color: #0000ff;">in</span><span style="color: #000000;"> dataList)
                {
                    sb.Append(</span><span style="color: #800000;">"</span><span style="color: #800000;">(</span><span style="color: #800000;">"</span><span style="color: #000000;">);
                    </span><span style="color: #0000ff;">for</span>(<span style="color: #0000ff;">int</span> j = <span style="color: #800080;">0</span> ; j &lt; properotyList.Count ; j++<span style="color: #000000;">)
                    {
                        PropertyInfo properotyInfo </span>= item.GetType().GetProperty(properotyList[ j ]); <span style="color: #008000;">//</span><span style="color: #008000;"> 属性的信息</span>
                        <span style="color: #0000ff;">object</span> properotyValue = properotyInfo.GetValue(item , <span style="color: #0000ff;">null</span>);<span style="color: #008000;">//</span><span style="color: #008000;"> 属性的值</span>
                        <span style="color: #0000ff;">string</span> cellValue = properotyValue == <span style="color: #0000ff;">null</span> ? <span style="color: #800000;">""</span> : properotyValue.ToString();<span style="color: #008000;">//</span><span style="color: #008000;"> 单元格的值</span>
                        sb.Append(<span style="color: #800000;">"</span><span style="color: #800000;">\"</span><span style="color: #800000;">"</span><span style="color: #000000;">);
                        sb.Append(properotyValue);
                        sb.Append(</span><span style="color: #800000;">"</span><span style="color: #800000;">\"</span><span style="color: #800000;">"</span><span style="color: #000000;">);
                        </span><span style="color: #0000ff;">if</span>(j &lt; properotyList.Count - <span style="color: #800080;">1</span><span style="color: #000000;">)
                        {
                            sb.Append(</span><span style="color: #800000;">"</span><span style="color: #800000;">,</span><span style="color: #800000;">"</span><span style="color: #000000;">);
                        }
                    }
                    sb.Append(</span><span style="color: #800000;">"</span><span style="color: #800000;">)</span><span style="color: #800000;">"</span><span style="color: #000000;">);
                    </span><span style="color: #0000ff;">if</span>(i++ &lt; dataList.Count - <span style="color: #800080;">1</span><span style="color: #000000;">)
                    {
                        sb.Append(</span><span style="color: #800000;">"</span><span style="color: #800000;">,</span><span style="color: #800000;">"</span><span style="color: #000000;">);
                    }
                }
                sql </span>=<span style="color: #000000;"> sb.ToString();

                MySqlTransaction tran </span>=<span style="color: #000000;"> con.BeginTransaction();
                MySqlCommand commd </span>= <span style="color: #0000ff;">new</span><span style="color: #000000;"> MySqlCommand(sql , con , tran);
                </span><span style="color: #0000ff;">try</span><span style="color: #000000;">
                {
                    </span><span style="color: #0000ff;">int</span> query =<span style="color: #000000;"> commd.ExecuteNonQuery();
                    result </span>= <span style="color: #0000ff;">true</span><span style="color: #000000;">;
                }
                </span><span style="color: #0000ff;">catch</span><span style="color: #000000;">(Exception e)
                {
                    tran.Rollback();
                    </span><span style="color: #0000ff;">throw</span><span style="color: #000000;">;
                }
                </span><span style="color: #0000ff;">return</span><span style="color: #000000;"> result;
            }
        }




        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;summary&gt;</span>
        <span style="color: #808080;">///</span><span style="color: #008000;"> DataReader To List
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;/summary&gt;</span>
        <span style="color: #808080;">///</span> <span style="color: #808080;">&lt;typeparam name="T"&gt;&lt;/typeparam&gt;</span>
        <span style="color: #808080;">///</span> <span style="color: #808080;">&lt;param name="reader"&gt;&lt;/param&gt;</span>
        <span style="color: #808080;">///</span> <span style="color: #808080;">&lt;returns&gt;&lt;/returns&gt;</span>
        <span style="color: #0000ff;">private</span> <span style="color: #0000ff;">static</span> List&lt;T&gt; DataReaderToList&lt;T&gt;(MySqlDataReader reader) <span style="color: #0000ff;">where</span> T : <span style="color: #0000ff;">new</span><span style="color: #000000;">()
        {
            List</span>&lt;T&gt; list = <span style="color: #0000ff;">new</span> List&lt;T&gt;<span style="color: #000000;">();
            </span><span style="color: #0000ff;">if</span><span style="color: #000000;">(reader.HasRows)
            {
                </span><span style="color: #0000ff;">while</span><span style="color: #000000;">(reader.Read())
                {
                    T t </span>= <span style="color: #0000ff;">new</span><span style="color: #000000;"> T();
                    Type type </span>=<span style="color: #000000;"> t.GetType();
                    </span><span style="color: #0000ff;">var</span> properties =<span style="color: #000000;"> type.GetProperties();
                    </span><span style="color: #0000ff;">foreach</span>(<span style="color: #0000ff;">var</span> item <span style="color: #0000ff;">in</span><span style="color: #000000;"> properties)
                    {
                        </span><span style="color: #0000ff;">string</span> name =<span style="color: #000000;"> item.Name;
                        reader.GetSchemaTable().DefaultView.RowFilter </span>= <span style="color: #800000;">"</span><span style="color: #800000;">ColumnName= '</span><span style="color: #800000;">"</span> + name + <span style="color: #800000;">"</span><span style="color: #800000;">'</span><span style="color: #800000;">"</span><span style="color: #000000;">;
                        </span><span style="color: #0000ff;">bool</span> check = reader.GetSchemaTable().DefaultView.Count &gt; <span style="color: #800080;">0</span><span style="color: #000000;">;
                        </span><span style="color: #0000ff;">if</span><span style="color: #000000;">(check)
                        {
                            </span><span style="color: #0000ff;">if</span>(!<span style="color: #000000;">item.CanWrite)
                            {
                                </span><span style="color: #0000ff;">continue</span><span style="color: #000000;">;
                            }
                            </span><span style="color: #0000ff;">var</span> value =<span style="color: #000000;"> reader[ name ];
                            </span><span style="color: #0000ff;">if</span>(value !=<span style="color: #000000;"> DBNull.Value)
                            {
                                item.SetValue(t , value , </span><span style="color: #0000ff;">null</span><span style="color: #000000;">);
                            }
                        }
                    }
                    list.Add(t);
                }
            }
            </span><span style="color: #0000ff;">return</span><span style="color: #000000;"> list;
        }
    }
}</span></pre>
</div>
<p>&nbsp;</p>