<div class="cnblogs_code">
<pre><span style="color: #0000ff;">using</span><span style="color: #000000;"> System.Collections.Generic;
</span><span style="color: #0000ff;">using</span><span style="color: #000000;"> System.Data;
</span><span style="color: #0000ff;">using</span><span style="color: #000000;"> System.IO;
</span><span style="color: #0000ff;">using</span><span style="color: #000000;"> System.Linq;
</span><span style="color: #0000ff;">using</span><span style="color: #000000;"> System.Web;

</span><span style="color: #0000ff;">namespace</span><span style="color: #000000;"> MVCStudy.Helper
{
    </span><span style="color: #0000ff;">public</span> <span style="color: #0000ff;">class</span><span style="color: #000000;"> ExcelHandle
    {
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;summary&gt;</span> 
        <span style="color: #808080;">///</span><span style="color: #008000;"> 将DataTable数据导出到Excel文件中(xlsx) 
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;/summary&gt;</span> 
        <span style="color: #808080;">///</span> <span style="color: #808080;">&lt;param name="dt"&gt;</span><span style="color: #008000;">数据源</span><span style="color: #808080;">&lt;/param&gt;</span> 
        <span style="color: #808080;">///</span> <span style="color: #808080;">&lt;param name="excelName"&gt;</span><span style="color: #008000;">文件名称</span><span style="color: #808080;">&lt;/param&gt;</span> 
        <span style="color: #0000ff;">public</span> <span style="color: #0000ff;">static</span> <span style="color: #0000ff;">void</span> TableToExcelForXLSX(DataTable dt, <span style="color: #0000ff;">string</span><span style="color: #000000;"> excelName)
        {
            XSSFWorkbook xssfworkbook </span>= <span style="color: #0000ff;">new</span><span style="color: #000000;"> XSSFWorkbook();
            ISheet sheet </span>= xssfworkbook.CreateSheet(<span style="color: #800000;">"</span><span style="color: #800000;">Test</span><span style="color: #800000;">"</span><span style="color: #000000;">);
            </span><span style="color: #008000;">//</span><span style="color: #008000;">表头 </span>
            IRow row = sheet.CreateRow(<span style="color: #800080;">0</span><span style="color: #000000;">);
            </span><span style="color: #0000ff;">for</span> (<span style="color: #0000ff;">int</span> i = <span style="color: #800080;">0</span>; i &lt; dt.Columns.Count; i++<span style="color: #000000;">)
            {
                ICell cell </span>=<span style="color: #000000;"> row.CreateCell(i);
                cell.SetCellValue(dt.Columns[i].ColumnName);
            }
            </span><span style="color: #008000;">//</span><span style="color: #008000;">数据 </span>
            <span style="color: #0000ff;">for</span> (<span style="color: #0000ff;">int</span> i = <span style="color: #800080;">0</span>; i &lt; dt.Rows.Count; i++<span style="color: #000000;">)
            {
                IRow row1 </span>= sheet.CreateRow(i + <span style="color: #800080;">1</span><span style="color: #000000;">);
                </span><span style="color: #0000ff;">for</span> (<span style="color: #0000ff;">int</span> j = <span style="color: #800080;">0</span>; j &lt; dt.Columns.Count; j++<span style="color: #000000;">)
                {
                    ICell cell </span>=<span style="color: #000000;"> row1.CreateCell(j);
                    cell.SetCellValue(dt.Rows[i][j].ToString());
                }
            }
            HttpContext curContext </span>=<span style="color: #000000;"> HttpContext.Current;
            curContext.Response.Clear();
            curContext.Response.ContentType </span>= <span style="color: #800000;">"</span><span style="color: #800000;">application/x-excel</span><span style="color: #800000;">"</span><span style="color: #000000;">;
            </span><span style="color: #0000ff;">string</span> filename = HttpUtility.UrlEncode(excelName + DateTime.Now.ToString(<span style="color: #800000;">"</span><span style="color: #800000;">yyyyMMddHHmm</span><span style="color: #800000;">"</span>) + <span style="color: #800000;">"</span><span style="color: #800000;">.xlsx</span><span style="color: #800000;">"</span><span style="color: #000000;">);
            curContext.Response.AddHeader(</span><span style="color: #800000;">"</span><span style="color: #800000;">Content-Disposition</span><span style="color: #800000;">"</span>, <span style="color: #800000;">"</span><span style="color: #800000;">attachment;filename=</span><span style="color: #800000;">"</span> +<span style="color: #000000;"> filename);
            xssfworkbook.Write(curContext.Response.OutputStream);
            curContext.Response.End();
        }
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;summary&gt;</span>
        <span style="color: #808080;">///</span><span style="color: #008000;"> 读取excel（版本不低于2007）至DataTable
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;/summary&gt;</span>
        <span style="color: #808080;">///</span> <span style="color: #808080;">&lt;param name="file"&gt;&lt;/param&gt;</span>
        <span style="color: #808080;">///</span> <span style="color: #808080;">&lt;returns&gt;&lt;/returns&gt;</span>
        <span style="color: #0000ff;">public</span> <span style="color: #0000ff;">static</span> DataTable ExcelToTableForXLSX(<span style="color: #0000ff;">string</span><span style="color: #000000;"> file)
        {
            DataTable dt </span>= <span style="color: #0000ff;">new</span><span style="color: #000000;"> DataTable();
            </span><span style="color: #0000ff;">using</span> (FileStream fs = <span style="color: #0000ff;">new</span><span style="color: #000000;"> FileStream(file, FileMode.Open, FileAccess.Read))
            {
                XSSFWorkbook xssfworkbook </span>= <span style="color: #0000ff;">new</span><span style="color: #000000;"> XSSFWorkbook(fs);
                ISheet sheet </span>= xssfworkbook.GetSheetAt(<span style="color: #800080;">0</span><span style="color: #000000;">);
                </span><span style="color: #008000;">//</span><span style="color: #008000;">表头  </span>
                IRow header =<span style="color: #000000;"> sheet.GetRow(sheet.FirstRowNum);
                List</span>&lt;<span style="color: #0000ff;">int</span>&gt; columns = <span style="color: #0000ff;">new</span> List&lt;<span style="color: #0000ff;">int</span>&gt;<span style="color: #000000;">();
                </span><span style="color: #0000ff;">for</span> (<span style="color: #0000ff;">int</span> i = <span style="color: #800080;">0</span>; i &lt; header.LastCellNum; i++<span style="color: #000000;">)
                {
                    </span><span style="color: #0000ff;">object</span> obj = GetValueTypeForXLSX(header.GetCell(i) <span style="color: #0000ff;">as</span><span style="color: #000000;"> XSSFCell);
                    </span><span style="color: #0000ff;">if</span> (obj == <span style="color: #0000ff;">null</span> || obj.ToString() == <span style="color: #0000ff;">string</span><span style="color: #000000;">.Empty)
                    {
                        dt.Columns.Add(</span><span style="color: #0000ff;">new</span> DataColumn(<span style="color: #800000;">"</span><span style="color: #800000;">Columns</span><span style="color: #800000;">"</span> +<span style="color: #000000;"> i.ToString()));
                    }
                    </span><span style="color: #0000ff;">else</span><span style="color: #000000;">
                        dt.Columns.Add(</span><span style="color: #0000ff;">new</span><span style="color: #000000;"> DataColumn(obj.ToString()));
                    columns.Add(i);
                }
                </span><span style="color: #008000;">//</span><span style="color: #008000;">数据  </span>
                <span style="color: #0000ff;">for</span> (<span style="color: #0000ff;">int</span> i = sheet.FirstRowNum + <span style="color: #800080;">1</span>; i &lt;= sheet.LastRowNum; i++<span style="color: #000000;">)
                {
                    DataRow dr </span>=<span style="color: #000000;"> dt.NewRow();
                    </span><span style="color: #0000ff;">bool</span> hasValue = <span style="color: #0000ff;">false</span><span style="color: #000000;">;
                    </span><span style="color: #0000ff;">foreach</span> (<span style="color: #0000ff;">int</span> j <span style="color: #0000ff;">in</span><span style="color: #000000;"> columns)
                    {
                        dr[j] </span>= GetValueTypeForXLSX(sheet.GetRow(i).GetCell(j) <span style="color: #0000ff;">as</span><span style="color: #000000;"> XSSFCell);
                        </span><span style="color: #0000ff;">if</span> (dr[j] != <span style="color: #0000ff;">null</span> &amp;&amp; dr[j].ToString() != <span style="color: #0000ff;">string</span><span style="color: #000000;">.Empty)
                        {
                            hasValue </span>= <span style="color: #0000ff;">true</span><span style="color: #000000;">;
                        }
                    }
                    </span><span style="color: #0000ff;">if</span><span style="color: #000000;"> (hasValue)
                    {
                        dt.Rows.Add(dr);
                    }
                }
            }
            </span><span style="color: #0000ff;">return</span><span style="color: #000000;"> dt;
        }

        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;summary&gt;</span>  
        <span style="color: #808080;">///</span><span style="color: #008000;"> 获取单元格类型(xlsx)  
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;/summary&gt;</span>  
        <span style="color: #808080;">///</span> <span style="color: #808080;">&lt;param name="cell"&gt;&lt;/param&gt;</span>  
        <span style="color: #808080;">///</span> <span style="color: #808080;">&lt;returns&gt;&lt;/returns&gt;</span>  
        <span style="color: #0000ff;">private</span> <span style="color: #0000ff;">static</span> <span style="color: #0000ff;">object</span><span style="color: #000000;"> GetValueTypeForXLSX(XSSFCell cell)
        {
            </span><span style="color: #0000ff;">if</span> (cell == <span style="color: #0000ff;">null</span><span style="color: #000000;">)
                </span><span style="color: #0000ff;">return</span> <span style="color: #0000ff;">null</span><span style="color: #000000;">;
            </span><span style="color: #0000ff;">switch</span><span style="color: #000000;"> (cell.CellType)
            {
                </span><span style="color: #0000ff;">case</span> CellType.Blank: <span style="color: #008000;">//</span><span style="color: #008000;">BLANK:  </span>
                    <span style="color: #0000ff;">return</span> <span style="color: #0000ff;">null</span><span style="color: #000000;">;
                </span><span style="color: #0000ff;">case</span> CellType.Boolean: <span style="color: #008000;">//</span><span style="color: #008000;">BOOLEAN:  </span>
                    <span style="color: #0000ff;">return</span><span style="color: #000000;"> cell.BooleanCellValue;
                </span><span style="color: #0000ff;">case</span> CellType.Numeric: <span style="color: #008000;">//</span><span style="color: #008000;">NUMERIC:  </span>
                    <span style="color: #0000ff;">return</span><span style="color: #000000;"> cell.NumericCellValue;
                </span><span style="color: #0000ff;">case</span> CellType.String: <span style="color: #008000;">//</span><span style="color: #008000;">STRING:  </span>
                    <span style="color: #0000ff;">return</span><span style="color: #000000;"> cell.StringCellValue;
                </span><span style="color: #0000ff;">case</span> CellType.Error: <span style="color: #008000;">//</span><span style="color: #008000;">ERROR:  </span>
                    <span style="color: #0000ff;">return</span><span style="color: #000000;"> cell.ErrorCellValue;
                </span><span style="color: #0000ff;">case</span> CellType.Formula: <span style="color: #008000;">//</span><span style="color: #008000;">FORMULA:  </span>
                <span style="color: #0000ff;">default</span><span style="color: #000000;">:
                    </span><span style="color: #0000ff;">return</span> <span style="color: #800000;">"</span><span style="color: #800000;">=</span><span style="color: #800000;">"</span> +<span style="color: #000000;"> cell.CellFormula;
            }
        }

        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;summary&gt;</span>
        <span style="color: #808080;">///</span><span style="color: #008000;"> 将List导出到Excel
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;/summary&gt;</span>
        <span style="color: #808080;">///</span> <span style="color: #808080;">&lt;param name="enList"&gt;</span><span style="color: #008000;">List数据</span><span style="color: #808080;">&lt;/param&gt;</span>
        <span style="color: #808080;">///</span> <span style="color: #808080;">&lt;param name="fileName"&gt;</span><span style="color: #008000;">Excel文件名</span><span style="color: #808080;">&lt;/param&gt;</span>
        <span style="color: #808080;">///</span> <span style="color: #808080;">&lt;param name="sheetName"&gt;</span><span style="color: #008000;">Excel工作表名</span><span style="color: #808080;">&lt;/param&gt;</span>
        <span style="color: #808080;">///</span> <span style="color: #808080;">&lt;param name="cell0Value"&gt;</span><span style="color: #008000;">首行提示信息</span><span style="color: #808080;">&lt;/param&gt;</span>
        <span style="color: #0000ff;">public</span> <span style="color: #0000ff;">void</span> IListToExcel( IList enList, <span style="color: #0000ff;">string</span> fileName, <span style="color: #0000ff;">string</span> sheetName,<span style="color: #0000ff;">string</span><span style="color: #000000;"> cell0Value)
        {
            IWorkbook workbook </span>= <span style="color: #0000ff;">new</span><span style="color: #000000;"> XSSFWorkbook();
            ISheet sheet </span>=<span style="color: #000000;"> workbook.CreateSheet(sheetName);
            sheet.CreateRow(</span><span style="color: #800080;">0</span>).CreateCell(<span style="color: #800080;">0</span><span style="color: #000000;">).SetCellValue(cell0Value);
            List</span>&lt;<span style="color: #0000ff;">string</span>&gt; Ihead = <span style="color: #0000ff;">new</span> List&lt;<span style="color: #0000ff;">string</span>&gt;<span style="color: #000000;">();
            Type types </span>= enList[<span style="color: #800080;">0</span><span style="color: #000000;">].GetType();
            </span><span style="color: #0000ff;">foreach</span>(<span style="color: #0000ff;">var</span> item <span style="color: #0000ff;">in</span><span style="color: #000000;"> types.GetProperties())
            {
                Ihead.Add(item.Name);
            }
            IRow row1 </span>= sheet.CreateRow(<span style="color: #800080;">1</span><span style="color: #000000;">);
            </span><span style="color: #0000ff;">for</span> (<span style="color: #0000ff;">int</span> i = <span style="color: #800080;">0</span>; i &lt; Ihead.Count; i++<span style="color: #000000;">)
            {
                row1.CreateCell(i).SetCellValue(Ihead[i]);
            }
            </span><span style="color: #0000ff;">int</span> rowIndex = <span style="color: #800080;">2</span><span style="color: #000000;">;
            </span><span style="color: #0000ff;">foreach</span> (<span style="color: #0000ff;">var</span> item <span style="color: #0000ff;">in</span><span style="color: #000000;"> enList)
            {
                IRow rowTmp </span>=<span style="color: #000000;"> sheet.CreateRow(rowIndex);
                </span><span style="color: #0000ff;">for</span> (<span style="color: #0000ff;">int</span> i = <span style="color: #800080;">0</span>; i &lt; Ihead.Count; i++<span style="color: #000000;">)
                {
                    System.Reflection.PropertyInfo properotyInfo </span>= item.GetType().GetProperty(Ihead[i]); <span style="color: #008000;">//</span><span style="color: #008000;"> 属性的信息</span>
                    <span style="color: #0000ff;">object</span> properotyValue = properotyInfo.GetValue(item, <span style="color: #0000ff;">null</span>);<span style="color: #008000;">//</span><span style="color: #008000;"> 属性的值</span>
                    <span style="color: #0000ff;">string</span> cellValue = properotyValue.ToString()??<span style="color: #0000ff;">null</span>;<span style="color: #008000;">//</span><span style="color: #008000;"> 单元格的值</span>
<span style="color: #000000;">                    rowTmp.CreateCell(i).SetCellValue(cellValue);
                }
                rowIndex</span>++<span style="color: #000000;">;
            }
            </span><span style="color: #0000ff;">using</span> (FileStream file = <span style="color: #0000ff;">new</span><span style="color: #000000;"> FileStream(fileName, FileMode.Create))
            {
                workbook.Write(file);
                file.Close();
            }
        }
    }
}</span></pre>
</div>
<p>&nbsp;</p>