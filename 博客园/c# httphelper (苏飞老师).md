<div class="cnblogs_code">
<pre><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;summary&gt;</span>
<span style="color: #808080;">///</span><span style="color: #008000;"> 类说明：HttpHelper类，用来实现Http访问，Post或者Get方式的，直接访问，带Cookie的，带证书的等方式，可以设置代理
</span><span style="color: #808080;">///</span><span style="color: #008000;"> 重要提示：请不要自行修改本类，如果因为你自己修改后将无法升级到新版本。如果确实有什么问题请到官方网站提建议，
</span><span style="color: #808080;">///</span><span style="color: #008000;"> 我们一定会及时修改
</span><span style="color: #808080;">///</span><span style="color: #008000;"> 编码日期：2011-09-20
</span><span style="color: #808080;">///</span><span style="color: #008000;"> 编 码 人：苏飞
</span><span style="color: #808080;">///</span><span style="color: #008000;"> 联系方式：361983679  
</span><span style="color: #808080;">///</span><span style="color: #008000;"> 官方网址：</span><span style="color: #008000; text-decoration: underline;">http://www.sufeinet.com/thread-3-1-1.html</span>
<span style="color: #808080;">///</span><span style="color: #008000;"> 修改日期：2017-09-30
</span><span style="color: #808080;">///</span><span style="color: #008000;"> 版 本 号：1.9
</span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;/summary&gt;</span>
<span style="color: #0000ff;">using</span><span style="color: #000000;"> System;
</span><span style="color: #0000ff;">using</span><span style="color: #000000;"> System.Collections.Generic;
</span><span style="color: #0000ff;">using</span><span style="color: #000000;"> System.Text;
</span><span style="color: #0000ff;">using</span><span style="color: #000000;"> System.Net;
</span><span style="color: #0000ff;">using</span><span style="color: #000000;"> System.IO;
</span><span style="color: #0000ff;">using</span><span style="color: #000000;"> System.Text.RegularExpressions;
</span><span style="color: #0000ff;">using</span><span style="color: #000000;"> System.IO.Compression;
</span><span style="color: #0000ff;">using</span><span style="color: #000000;"> System.Security.Cryptography.X509Certificates;
</span><span style="color: #0000ff;">using</span><span style="color: #000000;"> System.Net.Security;
</span><span style="color: #0000ff;">using</span><span style="color: #000000;"> System.Linq;
</span><span style="color: #0000ff;">using</span><span style="color: #000000;"> System.Net.Cache;

</span><span style="color: #0000ff;">namespace</span><span style="color: #000000;"> XiCheng.Hotel
{
    </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;summary&gt;</span>
    <span style="color: #808080;">///</span><span style="color: #008000;"> Http连接操作帮助类
    </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;/summary&gt;</span>
    <span style="color: #0000ff;">public</span> <span style="color: #0000ff;">class</span><span style="color: #000000;"> HttpHelper
    {
        </span><span style="color: #0000ff;">#region</span> 预定义方变量
        <span style="color: #008000;">//</span><span style="color: #008000;">默认的编码</span>
        <span style="color: #0000ff;">private</span> Encoding encoding =<span style="color: #000000;"> Encoding.Default;
        </span><span style="color: #008000;">//</span><span style="color: #008000;">Post数据编码</span>
        <span style="color: #0000ff;">private</span> Encoding postencoding =<span style="color: #000000;"> Encoding.Default;
        </span><span style="color: #008000;">//</span><span style="color: #008000;">HttpWebRequest对象用来发起请求</span>
        <span style="color: #0000ff;">private</span> HttpWebRequest request = <span style="color: #0000ff;">null</span><span style="color: #000000;">;
        </span><span style="color: #008000;">//</span><span style="color: #008000;">获取影响流的数据对象</span>
        <span style="color: #0000ff;">private</span> HttpWebResponse response = <span style="color: #0000ff;">null</span><span style="color: #000000;">;
        </span><span style="color: #008000;">//</span><span style="color: #008000;">设置本地的出口ip和端口</span>
        <span style="color: #0000ff;">private</span> IPEndPoint _IPEndPoint = <span style="color: #0000ff;">null</span><span style="color: #000000;">;
        </span><span style="color: #0000ff;">#endregion</span>

        <span style="color: #0000ff;">#region</span> Public

        <span style="color: #808080;">///</span> <span style="color: #808080;">&lt;summary&gt;</span>
        <span style="color: #808080;">///</span><span style="color: #008000;"> 根据相传入的数据，得到相应页面数据
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;/summary&gt;</span>
        <span style="color: #808080;">///</span> <span style="color: #808080;">&lt;param name="item"&gt;</span><span style="color: #008000;">参数类对象</span><span style="color: #808080;">&lt;/param&gt;</span>
        <span style="color: #808080;">///</span> <span style="color: #808080;">&lt;returns&gt;</span><span style="color: #008000;">返回HttpResult类型</span><span style="color: #808080;">&lt;/returns&gt;</span>
        <span style="color: #0000ff;">public</span><span style="color: #000000;"> HttpResult GetHtml(HttpItem item)
        {
            </span><span style="color: #008000;">//</span><span style="color: #008000;">返回参数</span>
            HttpResult result = <span style="color: #0000ff;">new</span><span style="color: #000000;"> HttpResult();
            </span><span style="color: #0000ff;">try</span><span style="color: #000000;">
            {
                </span><span style="color: #008000;">//</span><span style="color: #008000;">准备参数</span>
<span style="color: #000000;">                SetRequest(item);
            }
            </span><span style="color: #0000ff;">catch</span><span style="color: #000000;"> (Exception ex)
            {
                </span><span style="color: #008000;">//</span><span style="color: #008000;">配置参数时出错</span>
                <span style="color: #0000ff;">return</span> <span style="color: #0000ff;">new</span> HttpResult() { Cookie = <span style="color: #0000ff;">string</span>.Empty, Header = <span style="color: #0000ff;">null</span>, Html = ex.Message, StatusDescription = <span style="color: #800000;">"</span><span style="color: #800000;">配置参数时出错：</span><span style="color: #800000;">"</span> +<span style="color: #000000;"> ex.Message };
            }
            </span><span style="color: #0000ff;">try</span><span style="color: #000000;">
            {
                </span><span style="color: #008000;">//</span><span style="color: #008000;">请求数据</span>
                <span style="color: #0000ff;">using</span> (response =<span style="color: #000000;"> (HttpWebResponse)request.GetResponse())
                {
                    GetData(item, result);
                }
            }
            </span><span style="color: #0000ff;">catch</span><span style="color: #000000;"> (WebException ex)
            {
                </span><span style="color: #0000ff;">if</span> (ex.Response != <span style="color: #0000ff;">null</span><span style="color: #000000;">)
                {
                    </span><span style="color: #0000ff;">using</span> (response =<span style="color: #000000;"> (HttpWebResponse)ex.Response)
                    {
                        GetData(item, result);
                    }
                }
                </span><span style="color: #0000ff;">else</span><span style="color: #000000;">
                {
                    result.Html </span>=<span style="color: #000000;"> ex.Message;
                }
            }
            </span><span style="color: #0000ff;">catch</span><span style="color: #000000;"> (Exception ex)
            {
                result.Html </span>=<span style="color: #000000;"> ex.Message;
            }
            </span><span style="color: #0000ff;">if</span> (item.IsToLower) result.Html =<span style="color: #000000;"> result.Html.ToLower();
            </span><span style="color: #008000;">//</span><span style="color: #008000;">重置request，response为空</span>
            <span style="color: #0000ff;">if</span><span style="color: #000000;"> (item.IsReset)
            {
                request </span>= <span style="color: #0000ff;">null</span><span style="color: #000000;">;
                response </span>= <span style="color: #0000ff;">null</span><span style="color: #000000;">;
            }
            </span><span style="color: #0000ff;">return</span><span style="color: #000000;"> result;
        }
        </span><span style="color: #0000ff;">#endregion</span>

        <span style="color: #0000ff;">#region</span> GetData

        <span style="color: #808080;">///</span> <span style="color: #808080;">&lt;summary&gt;</span>
        <span style="color: #808080;">///</span><span style="color: #008000;"> 获取数据的并解析的方法
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;/summary&gt;</span>
        <span style="color: #808080;">///</span> <span style="color: #808080;">&lt;param name="item"&gt;&lt;/param&gt;</span>
        <span style="color: #808080;">///</span> <span style="color: #808080;">&lt;param name="result"&gt;&lt;/param&gt;</span>
        <span style="color: #0000ff;">private</span> <span style="color: #0000ff;">void</span><span style="color: #000000;"> GetData(HttpItem item, HttpResult result)
        {
            </span><span style="color: #0000ff;">if</span> (response == <span style="color: #0000ff;">null</span><span style="color: #000000;">)
            {
                </span><span style="color: #0000ff;">return</span><span style="color: #000000;">;
            }
            </span><span style="color: #0000ff;">#region</span> base
            <span style="color: #008000;">//</span><span style="color: #008000;">获取StatusCode</span>
            result.StatusCode =<span style="color: #000000;"> response.StatusCode;
            </span><span style="color: #008000;">//</span><span style="color: #008000;">获取StatusDescription</span>
            result.StatusDescription =<span style="color: #000000;"> response.StatusDescription;
            </span><span style="color: #008000;">//</span><span style="color: #008000;">获取Headers</span>
            result.Header =<span style="color: #000000;"> response.Headers;
            </span><span style="color: #008000;">//</span><span style="color: #008000;">获取最后访问的URl</span>
            result.ResponseUri =<span style="color: #000000;"> response.ResponseUri.ToString();
            </span><span style="color: #008000;">//</span><span style="color: #008000;">获取CookieCollection</span>
            <span style="color: #0000ff;">if</span> (response.Cookies != <span style="color: #0000ff;">null</span>) result.CookieCollection =<span style="color: #000000;"> response.Cookies;
            </span><span style="color: #008000;">//</span><span style="color: #008000;">获取set-cookie</span>
            <span style="color: #0000ff;">if</span> (response.Headers[<span style="color: #800000;">"</span><span style="color: #800000;">set-cookie</span><span style="color: #800000;">"</span>] != <span style="color: #0000ff;">null</span>) result.Cookie = response.Headers[<span style="color: #800000;">"</span><span style="color: #800000;">set-cookie</span><span style="color: #800000;">"</span><span style="color: #000000;">];
            </span><span style="color: #0000ff;">#endregion</span>

            <span style="color: #0000ff;">#region</span> byte

            <span style="color: #008000;">//</span><span style="color: #008000;">处理网页Byte</span>
            <span style="color: #0000ff;">byte</span>[] ResponseByte =<span style="color: #000000;"> GetByte();

            </span><span style="color: #0000ff;">#endregion</span>

            <span style="color: #0000ff;">#region</span> Html
            <span style="color: #0000ff;">if</span> (ResponseByte != <span style="color: #0000ff;">null</span> &amp;&amp; ResponseByte.Length &gt; <span style="color: #800080;">0</span><span style="color: #000000;">)
            {
                </span><span style="color: #008000;">//</span><span style="color: #008000;">设置编码</span>
<span style="color: #000000;">                SetEncoding(item, result, ResponseByte);
                </span><span style="color: #008000;">//</span><span style="color: #008000;">得到返回的HTML</span>
                result.Html =<span style="color: #000000;"> encoding.GetString(ResponseByte);
            }
            </span><span style="color: #0000ff;">else</span><span style="color: #000000;">
            {
                </span><span style="color: #008000;">//</span><span style="color: #008000;">没有返回任何Html代码</span>
                result.Html = <span style="color: #0000ff;">string</span><span style="color: #000000;">.Empty;
            }
            </span><span style="color: #0000ff;">#endregion</span><span style="color: #000000;">
        }
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;summary&gt;</span>
        <span style="color: #808080;">///</span><span style="color: #008000;"> 设置编码
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;/summary&gt;</span>
        <span style="color: #808080;">///</span> <span style="color: #808080;">&lt;param name="item"&gt;</span><span style="color: #008000;">HttpItem</span><span style="color: #808080;">&lt;/param&gt;</span>
        <span style="color: #808080;">///</span> <span style="color: #808080;">&lt;param name="result"&gt;</span><span style="color: #008000;">HttpResult</span><span style="color: #808080;">&lt;/param&gt;</span>
        <span style="color: #808080;">///</span> <span style="color: #808080;">&lt;param name="ResponseByte"&gt;</span><span style="color: #008000;">byte[]</span><span style="color: #808080;">&lt;/param&gt;</span>
        <span style="color: #0000ff;">private</span> <span style="color: #0000ff;">void</span> SetEncoding(HttpItem item, HttpResult result, <span style="color: #0000ff;">byte</span><span style="color: #000000;">[] ResponseByte)
        {
            </span><span style="color: #008000;">//</span><span style="color: #008000;">是否返回Byte类型数据</span>
            <span style="color: #0000ff;">if</span> (item.ResultType == ResultType.Byte) result.ResultByte =<span style="color: #000000;"> ResponseByte;
            </span><span style="color: #008000;">//</span><span style="color: #008000;">从这里开始我们要无视编码了</span>
            <span style="color: #0000ff;">if</span> (encoding == <span style="color: #0000ff;">null</span><span style="color: #000000;">)
            {
                Match meta </span>= Regex.Match(Encoding.Default.GetString(ResponseByte), <span style="color: #800000;">"</span><span style="color: #800000;">&lt;meta[^&lt;]*charset=([^&lt;]*)[\"']</span><span style="color: #800000;">"</span><span style="color: #000000;">, RegexOptions.IgnoreCase);
                </span><span style="color: #0000ff;">string</span> c = <span style="color: #0000ff;">string</span><span style="color: #000000;">.Empty;
                </span><span style="color: #0000ff;">if</span> (meta != <span style="color: #0000ff;">null</span> &amp;&amp; meta.Groups.Count &gt; <span style="color: #800080;">0</span><span style="color: #000000;">)
                {
                    c </span>= meta.Groups[<span style="color: #800080;">1</span><span style="color: #000000;">].Value.ToLower().Trim();
                }
                </span><span style="color: #0000ff;">if</span> (c.Length &gt; <span style="color: #800080;">2</span><span style="color: #000000;">)
                {
                    </span><span style="color: #0000ff;">try</span><span style="color: #000000;">
                    {
                        encoding </span>= Encoding.GetEncoding(c.Replace(<span style="color: #800000;">"</span><span style="color: #800000;">\"</span><span style="color: #800000;">"</span>, <span style="color: #0000ff;">string</span>.Empty).Replace(<span style="color: #800000;">"</span><span style="color: #800000;">'</span><span style="color: #800000;">"</span>, <span style="color: #800000;">""</span>).Replace(<span style="color: #800000;">"</span><span style="color: #800000;">;</span><span style="color: #800000;">"</span>, <span style="color: #800000;">""</span>).Replace(<span style="color: #800000;">"</span><span style="color: #800000;">iso-8859-1</span><span style="color: #800000;">"</span>, <span style="color: #800000;">"</span><span style="color: #800000;">gbk</span><span style="color: #800000;">"</span><span style="color: #000000;">).Trim());
                    }
                    </span><span style="color: #0000ff;">catch</span><span style="color: #000000;">
                    {
                        </span><span style="color: #0000ff;">if</span> (<span style="color: #0000ff;">string</span><span style="color: #000000;">.IsNullOrEmpty(response.CharacterSet))
                        {
                            encoding </span>=<span style="color: #000000;"> Encoding.UTF8;
                        }
                        </span><span style="color: #0000ff;">else</span><span style="color: #000000;">
                        {
                            encoding </span>=<span style="color: #000000;"> Encoding.GetEncoding(response.CharacterSet);
                        }
                    }
                }
                </span><span style="color: #0000ff;">else</span><span style="color: #000000;">
                {
                    </span><span style="color: #0000ff;">if</span> (<span style="color: #0000ff;">string</span><span style="color: #000000;">.IsNullOrEmpty(response.CharacterSet))
                    {
                        encoding </span>=<span style="color: #000000;"> Encoding.UTF8;
                    }
                    </span><span style="color: #0000ff;">else</span><span style="color: #000000;">
                    {
                        encoding </span>=<span style="color: #000000;"> Encoding.GetEncoding(response.CharacterSet);
                    }
                }
            }
        }
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;summary&gt;</span>
        <span style="color: #808080;">///</span><span style="color: #008000;"> 提取网页Byte
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;/summary&gt;</span>
        <span style="color: #808080;">///</span> <span style="color: #808080;">&lt;returns&gt;&lt;/returns&gt;</span>
        <span style="color: #0000ff;">private</span> <span style="color: #0000ff;">byte</span><span style="color: #000000;">[] GetByte()
        {
            </span><span style="color: #0000ff;">byte</span>[] ResponseByte = <span style="color: #0000ff;">null</span><span style="color: #000000;">;
            </span><span style="color: #0000ff;">using</span> (MemoryStream _stream = <span style="color: #0000ff;">new</span><span style="color: #000000;"> MemoryStream())
            {
                </span><span style="color: #008000;">//</span><span style="color: #008000;">GZIIP处理</span>
                <span style="color: #0000ff;">if</span> (response.ContentEncoding != <span style="color: #0000ff;">null</span> &amp;&amp; response.ContentEncoding.Equals(<span style="color: #800000;">"</span><span style="color: #800000;">gzip</span><span style="color: #800000;">"</span><span style="color: #000000;">, StringComparison.InvariantCultureIgnoreCase))
                {
                    </span><span style="color: #008000;">//</span><span style="color: #008000;">开始读取流并设置编码方式</span>
                    <span style="color: #0000ff;">new</span> GZipStream(response.GetResponseStream(), CompressionMode.Decompress).CopyTo(_stream, <span style="color: #800080;">10240</span><span style="color: #000000;">);
                }
                </span><span style="color: #0000ff;">else</span><span style="color: #000000;">
                {
                    </span><span style="color: #008000;">//</span><span style="color: #008000;">开始读取流并设置编码方式</span>
                    response.GetResponseStream().CopyTo(_stream, <span style="color: #800080;">10240</span><span style="color: #000000;">);
                }
                </span><span style="color: #008000;">//</span><span style="color: #008000;">获取Byte</span>
                ResponseByte =<span style="color: #000000;"> _stream.ToArray();
            }
            </span><span style="color: #0000ff;">return</span><span style="color: #000000;"> ResponseByte;
        }


        </span><span style="color: #0000ff;">#endregion</span>

        <span style="color: #0000ff;">#region</span> SetRequest

        <span style="color: #808080;">///</span> <span style="color: #808080;">&lt;summary&gt;</span>
        <span style="color: #808080;">///</span><span style="color: #008000;"> 为请求准备参数
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;/summary&gt;</span>
        <span style="color: #808080;">///</span><span style="color: #808080;">&lt;param name="item"&gt;</span><span style="color: #008000;">参数列表</span><span style="color: #808080;">&lt;/param&gt;</span>
        <span style="color: #0000ff;">private</span> <span style="color: #0000ff;">void</span><span style="color: #000000;"> SetRequest(HttpItem item)
        {

            </span><span style="color: #008000;">//</span><span style="color: #008000;"> 验证证书</span>
<span style="color: #000000;">            SetCer(item);
            </span><span style="color: #0000ff;">if</span> (item.IPEndPoint != <span style="color: #0000ff;">null</span><span style="color: #000000;">)
            {
                _IPEndPoint </span>=<span style="color: #000000;"> item.IPEndPoint;
                </span><span style="color: #008000;">//</span><span style="color: #008000;">设置本地的出口ip和端口</span>
                request.ServicePoint.BindIPEndPointDelegate = <span style="color: #0000ff;">new</span><span style="color: #000000;"> BindIPEndPoint(BindIPEndPointCallback);
            }
            </span><span style="color: #008000;">//</span><span style="color: #008000;">设置Header参数</span>
            <span style="color: #0000ff;">if</span> (item.Header != <span style="color: #0000ff;">null</span> &amp;&amp; item.Header.Count &gt; <span style="color: #800080;">0</span>) <span style="color: #0000ff;">foreach</span> (<span style="color: #0000ff;">string</span> key <span style="color: #0000ff;">in</span><span style="color: #000000;"> item.Header.AllKeys)
                {
                    request.Headers.Add(key, item.Header[key]);
                }
            </span><span style="color: #008000;">//</span><span style="color: #008000;"> 设置代理</span>
<span style="color: #000000;">            SetProxy(item);
            </span><span style="color: #0000ff;">if</span> (item.ProtocolVersion != <span style="color: #0000ff;">null</span>) request.ProtocolVersion =<span style="color: #000000;"> item.ProtocolVersion;
            request.ServicePoint.Expect100Continue </span>=<span style="color: #000000;"> item.Expect100Continue;
            </span><span style="color: #008000;">//</span><span style="color: #008000;">请求方式Get或者Post</span>
            request.Method =<span style="color: #000000;"> item.Method;
            request.Timeout </span>=<span style="color: #000000;"> item.Timeout;
            request.KeepAlive </span>=<span style="color: #000000;"> item.KeepAlive;
            request.ReadWriteTimeout </span>=<span style="color: #000000;"> item.ReadWriteTimeout;
            </span><span style="color: #0000ff;">if</span> (!<span style="color: #0000ff;">string</span><span style="color: #000000;">.IsNullOrWhiteSpace(item.Host))
            {
                request.Host </span>=<span style="color: #000000;"> item.Host;
            }
            </span><span style="color: #0000ff;">if</span> (item.IfModifiedSince != <span style="color: #0000ff;">null</span>) request.IfModifiedSince =<span style="color: #000000;"> Convert.ToDateTime(item.IfModifiedSince);
            </span><span style="color: #008000;">//</span><span style="color: #008000;">Accept</span>
            request.Accept =<span style="color: #000000;"> item.Accept;
            </span><span style="color: #008000;">//</span><span style="color: #008000;">ContentType返回类型</span>
            request.ContentType =<span style="color: #000000;"> item.ContentType;
            </span><span style="color: #008000;">//</span><span style="color: #008000;">UserAgent客户端的访问类型，包括浏览器版本和操作系统信息</span>
            request.UserAgent =<span style="color: #000000;"> item.UserAgent;
            </span><span style="color: #008000;">//</span><span style="color: #008000;"> 编码</span>
            encoding =<span style="color: #000000;"> item.Encoding;
            </span><span style="color: #008000;">//</span><span style="color: #008000;">设置安全凭证</span>
            request.Credentials =<span style="color: #000000;"> item.ICredentials;
            </span><span style="color: #008000;">//</span><span style="color: #008000;">设置Cookie</span>
<span style="color: #000000;">            SetCookie(item);
            </span><span style="color: #008000;">//</span><span style="color: #008000;">来源地址</span>
            request.Referer =<span style="color: #000000;"> item.Referer;
            </span><span style="color: #008000;">//</span><span style="color: #008000;">是否执行跳转功能</span>
            request.AllowAutoRedirect =<span style="color: #000000;"> item.Allowautoredirect;
            </span><span style="color: #0000ff;">if</span> (item.MaximumAutomaticRedirections &gt; <span style="color: #800080;">0</span><span style="color: #000000;">)
            {
                request.MaximumAutomaticRedirections </span>=<span style="color: #000000;"> item.MaximumAutomaticRedirections;
            }
            </span><span style="color: #008000;">//</span><span style="color: #008000;">设置Post数据</span>
<span style="color: #000000;">            SetPostData(item);
            </span><span style="color: #008000;">//</span><span style="color: #008000;">设置最大连接</span>
            <span style="color: #0000ff;">if</span> (item.Connectionlimit &gt; <span style="color: #800080;">0</span>) request.ServicePoint.ConnectionLimit =<span style="color: #000000;"> item.Connectionlimit;
        }
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;summary&gt;</span>
        <span style="color: #808080;">///</span><span style="color: #008000;"> 设置证书
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;/summary&gt;</span>
        <span style="color: #808080;">///</span> <span style="color: #808080;">&lt;param name="item"&gt;&lt;/param&gt;</span>
        <span style="color: #0000ff;">private</span> <span style="color: #0000ff;">void</span><span style="color: #000000;"> SetCer(HttpItem item)
        {
            </span><span style="color: #0000ff;">if</span> (!<span style="color: #0000ff;">string</span><span style="color: #000000;">.IsNullOrWhiteSpace(item.CerPath))
            {
                </span><span style="color: #008000;">//</span><span style="color: #008000;">这一句一定要写在创建连接的前面。使用回调的方法进行证书验证。</span>
                ServicePointManager.ServerCertificateValidationCallback = <span style="color: #0000ff;">new</span><span style="color: #000000;"> RemoteCertificateValidationCallback(CheckValidationResult);
                </span><span style="color: #008000;">//</span><span style="color: #008000;">初始化对像，并设置请求的URL地址</span>
                request =<span style="color: #000000;"> (HttpWebRequest)WebRequest.Create(item.URL);
                SetCerList(item);
                </span><span style="color: #008000;">//</span><span style="color: #008000;">将证书添加到请求里</span>
                request.ClientCertificates.Add(<span style="color: #0000ff;">new</span><span style="color: #000000;"> X509Certificate(item.CerPath));
            }
            </span><span style="color: #0000ff;">else</span><span style="color: #000000;">
            {
                </span><span style="color: #008000;">//</span><span style="color: #008000;">初始化对像，并设置请求的URL地址</span>
                request =<span style="color: #000000;"> (HttpWebRequest)WebRequest.Create(item.URL);
                SetCerList(item);
            }
        }
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;summary&gt;</span>
        <span style="color: #808080;">///</span><span style="color: #008000;"> 设置多个证书
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;/summary&gt;</span>
        <span style="color: #808080;">///</span> <span style="color: #808080;">&lt;param name="item"&gt;&lt;/param&gt;</span>
        <span style="color: #0000ff;">private</span> <span style="color: #0000ff;">void</span><span style="color: #000000;"> SetCerList(HttpItem item)
        {
            </span><span style="color: #0000ff;">if</span> (item.ClentCertificates != <span style="color: #0000ff;">null</span> &amp;&amp; item.ClentCertificates.Count &gt; <span style="color: #800080;">0</span><span style="color: #000000;">)
            {
                </span><span style="color: #0000ff;">foreach</span> (X509Certificate c <span style="color: #0000ff;">in</span><span style="color: #000000;"> item.ClentCertificates)
                {
                    request.ClientCertificates.Add(c);
                }
            }
        }
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;summary&gt;</span>
        <span style="color: #808080;">///</span><span style="color: #008000;"> 设置Cookie
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;/summary&gt;</span>
        <span style="color: #808080;">///</span> <span style="color: #808080;">&lt;param name="item"&gt;</span><span style="color: #008000;">Http参数</span><span style="color: #808080;">&lt;/param&gt;</span>
        <span style="color: #0000ff;">private</span> <span style="color: #0000ff;">void</span><span style="color: #000000;"> SetCookie(HttpItem item)
        {
            </span><span style="color: #0000ff;">if</span> (!<span style="color: #0000ff;">string</span>.IsNullOrEmpty(item.Cookie)) request.Headers[HttpRequestHeader.Cookie] =<span style="color: #000000;"> item.Cookie;
            </span><span style="color: #008000;">//</span><span style="color: #008000;">设置CookieCollection</span>
            <span style="color: #0000ff;">if</span> (item.ResultCookieType ==<span style="color: #000000;"> ResultCookieType.CookieCollection)
            {
                request.CookieContainer </span>= <span style="color: #0000ff;">new</span><span style="color: #000000;"> CookieContainer();
                </span><span style="color: #0000ff;">if</span> (item.CookieCollection != <span style="color: #0000ff;">null</span> &amp;&amp; item.CookieCollection.Count &gt; <span style="color: #800080;">0</span><span style="color: #000000;">)
                    request.CookieContainer.Add(item.CookieCollection);
            }
        }
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;summary&gt;</span>
        <span style="color: #808080;">///</span><span style="color: #008000;"> 设置Post数据
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;/summary&gt;</span>
        <span style="color: #808080;">///</span> <span style="color: #808080;">&lt;param name="item"&gt;</span><span style="color: #008000;">Http参数</span><span style="color: #808080;">&lt;/param&gt;</span>
        <span style="color: #0000ff;">private</span> <span style="color: #0000ff;">void</span><span style="color: #000000;"> SetPostData(HttpItem item)
        {
            </span><span style="color: #008000;">//</span><span style="color: #008000;">验证在得到结果时是否有传入数据</span>
            <span style="color: #0000ff;">if</span> (!request.Method.Trim().ToLower().Contains(<span style="color: #800000;">"</span><span style="color: #800000;">get</span><span style="color: #800000;">"</span><span style="color: #000000;">))
            {
                </span><span style="color: #0000ff;">if</span> (item.PostEncoding != <span style="color: #0000ff;">null</span><span style="color: #000000;">)
                {
                    postencoding </span>=<span style="color: #000000;"> item.PostEncoding;
                }
                </span><span style="color: #0000ff;">byte</span>[] buffer = <span style="color: #0000ff;">null</span><span style="color: #000000;">;
                </span><span style="color: #008000;">//</span><span style="color: #008000;">写入Byte类型</span>
                <span style="color: #0000ff;">if</span> (item.PostDataType == PostDataType.Byte &amp;&amp; item.PostdataByte != <span style="color: #0000ff;">null</span> &amp;&amp; item.PostdataByte.Length &gt; <span style="color: #800080;">0</span><span style="color: #000000;">)
                {
                    </span><span style="color: #008000;">//</span><span style="color: #008000;">验证在得到结果时是否有传入数据</span>
                    buffer =<span style="color: #000000;"> item.PostdataByte;
                }</span><span style="color: #008000;">//</span><span style="color: #008000;">写入文件</span>
                <span style="color: #0000ff;">else</span> <span style="color: #0000ff;">if</span> (item.PostDataType == PostDataType.FilePath &amp;&amp; !<span style="color: #0000ff;">string</span><span style="color: #000000;">.IsNullOrWhiteSpace(item.Postdata))
                {
                    StreamReader r </span>= <span style="color: #0000ff;">new</span><span style="color: #000000;"> StreamReader(item.Postdata, postencoding);
                    buffer </span>=<span style="color: #000000;"> postencoding.GetBytes(r.ReadToEnd());
                    r.Close();
                } </span><span style="color: #008000;">//</span><span style="color: #008000;">写入字符串</span>
                <span style="color: #0000ff;">else</span> <span style="color: #0000ff;">if</span> (!<span style="color: #0000ff;">string</span><span style="color: #000000;">.IsNullOrWhiteSpace(item.Postdata))
                {
                    buffer </span>=<span style="color: #000000;"> postencoding.GetBytes(item.Postdata);
                }
                </span><span style="color: #0000ff;">if</span> (buffer != <span style="color: #0000ff;">null</span><span style="color: #000000;">)
                {
                    request.ContentLength </span>=<span style="color: #000000;"> buffer.Length;
                    request.GetRequestStream().Write(buffer, </span><span style="color: #800080;">0</span><span style="color: #000000;">, buffer.Length);
                }
                </span><span style="color: #0000ff;">else</span><span style="color: #000000;">
                {
                    request.ContentLength </span>= <span style="color: #800080;">0</span><span style="color: #000000;">;
                }
            }
        }
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;summary&gt;</span>
        <span style="color: #808080;">///</span><span style="color: #008000;"> 设置代理
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;/summary&gt;</span>
        <span style="color: #808080;">///</span> <span style="color: #808080;">&lt;param name="item"&gt;</span><span style="color: #008000;">参数对象</span><span style="color: #808080;">&lt;/param&gt;</span>
        <span style="color: #0000ff;">private</span> <span style="color: #0000ff;">void</span><span style="color: #000000;"> SetProxy(HttpItem item)
        {
            </span><span style="color: #0000ff;">bool</span> isIeProxy = <span style="color: #0000ff;">false</span><span style="color: #000000;">;
            </span><span style="color: #0000ff;">if</span> (!<span style="color: #0000ff;">string</span><span style="color: #000000;">.IsNullOrWhiteSpace(item.ProxyIp))
            {
                isIeProxy </span>= item.ProxyIp.ToLower().Contains(<span style="color: #800000;">"</span><span style="color: #800000;">ieproxy</span><span style="color: #800000;">"</span><span style="color: #000000;">);
            }
            </span><span style="color: #0000ff;">if</span> (!<span style="color: #0000ff;">string</span>.IsNullOrWhiteSpace(item.ProxyIp) &amp;&amp; !<span style="color: #000000;">isIeProxy)
            {
                </span><span style="color: #008000;">//</span><span style="color: #008000;">设置代理服务器</span>
                <span style="color: #0000ff;">if</span> (item.ProxyIp.Contains(<span style="color: #800000;">"</span><span style="color: #800000;">:</span><span style="color: #800000;">"</span><span style="color: #000000;">))
                {
                    </span><span style="color: #0000ff;">string</span>[] plist = item.ProxyIp.Split(<span style="color: #800000;">'</span><span style="color: #800000;">:</span><span style="color: #800000;">'</span><span style="color: #000000;">);
                    WebProxy myProxy </span>= <span style="color: #0000ff;">new</span> WebProxy(plist[<span style="color: #800080;">0</span>].Trim(), Convert.ToInt32(plist[<span style="color: #800080;">1</span><span style="color: #000000;">].Trim()));
                    </span><span style="color: #008000;">//</span><span style="color: #008000;">建议连接</span>
                    myProxy.Credentials = <span style="color: #0000ff;">new</span><span style="color: #000000;"> NetworkCredential(item.ProxyUserName, item.ProxyPwd);
                    </span><span style="color: #008000;">//</span><span style="color: #008000;">给当前请求对象</span>
                    request.Proxy =<span style="color: #000000;"> myProxy;
                }
                </span><span style="color: #0000ff;">else</span><span style="color: #000000;">
                {
                    WebProxy myProxy </span>= <span style="color: #0000ff;">new</span> WebProxy(item.ProxyIp, <span style="color: #0000ff;">false</span><span style="color: #000000;">);
                    </span><span style="color: #008000;">//</span><span style="color: #008000;">建议连接</span>
                    myProxy.Credentials = <span style="color: #0000ff;">new</span><span style="color: #000000;"> NetworkCredential(item.ProxyUserName, item.ProxyPwd);
                    </span><span style="color: #008000;">//</span><span style="color: #008000;">给当前请求对象</span>
                    request.Proxy =<span style="color: #000000;"> myProxy;
                }
            }
            </span><span style="color: #0000ff;">else</span> <span style="color: #0000ff;">if</span><span style="color: #000000;"> (isIeProxy)
            {
                </span><span style="color: #008000;">//</span><span style="color: #008000;">设置为IE代理</span>
<span style="color: #000000;">            }
            </span><span style="color: #0000ff;">else</span><span style="color: #000000;">
            {
                request.Proxy </span>=<span style="color: #000000;"> item.WebProxy;
            }
        }


        </span><span style="color: #0000ff;">#endregion</span>

        <span style="color: #0000ff;">#region</span> private main
        <span style="color: #808080;">///</span> <span style="color: #808080;">&lt;summary&gt;</span>
        <span style="color: #808080;">///</span><span style="color: #008000;"> 回调验证证书问题
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;/summary&gt;</span>
        <span style="color: #808080;">///</span> <span style="color: #808080;">&lt;param name="sender"&gt;</span><span style="color: #008000;">流对象</span><span style="color: #808080;">&lt;/param&gt;</span>
        <span style="color: #808080;">///</span> <span style="color: #808080;">&lt;param name="certificate"&gt;</span><span style="color: #008000;">证书</span><span style="color: #808080;">&lt;/param&gt;</span>
        <span style="color: #808080;">///</span> <span style="color: #808080;">&lt;param name="chain"&gt;</span><span style="color: #008000;">X509Chain</span><span style="color: #808080;">&lt;/param&gt;</span>
        <span style="color: #808080;">///</span> <span style="color: #808080;">&lt;param name="errors"&gt;</span><span style="color: #008000;">SslPolicyErrors</span><span style="color: #808080;">&lt;/param&gt;</span>
        <span style="color: #808080;">///</span> <span style="color: #808080;">&lt;returns&gt;</span><span style="color: #008000;">bool</span><span style="color: #808080;">&lt;/returns&gt;</span>
        <span style="color: #0000ff;">private</span> <span style="color: #0000ff;">bool</span> CheckValidationResult(<span style="color: #0000ff;">object</span> sender, X509Certificate certificate, X509Chain chain, SslPolicyErrors errors) { <span style="color: #0000ff;">return</span> <span style="color: #0000ff;">true</span><span style="color: #000000;">; }

        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;summary&gt;</span>
        <span style="color: #808080;">///</span><span style="color: #008000;"> 通过设置这个属性，可以在发出连接的时候绑定客户端发出连接所使用的IP地址。 
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;/summary&gt;</span>
        <span style="color: #808080;">///</span> <span style="color: #808080;">&lt;param name="servicePoint"&gt;&lt;/param&gt;</span>
        <span style="color: #808080;">///</span> <span style="color: #808080;">&lt;param name="remoteEndPoint"&gt;&lt;/param&gt;</span>
        <span style="color: #808080;">///</span> <span style="color: #808080;">&lt;param name="retryCount"&gt;&lt;/param&gt;</span>
        <span style="color: #808080;">///</span> <span style="color: #808080;">&lt;returns&gt;&lt;/returns&gt;</span>
        <span style="color: #0000ff;">private</span> IPEndPoint BindIPEndPointCallback(ServicePoint servicePoint, IPEndPoint remoteEndPoint, <span style="color: #0000ff;">int</span><span style="color: #000000;"> retryCount)
        {
            </span><span style="color: #0000ff;">return</span> _IPEndPoint;<span style="color: #008000;">//</span><span style="color: #008000;">端口号</span>
<span style="color: #000000;">        }
        </span><span style="color: #0000ff;">#endregion</span><span style="color: #000000;">
    }

    </span><span style="color: #0000ff;">#region</span> public calss
    <span style="color: #808080;">///</span> <span style="color: #808080;">&lt;summary&gt;</span>
    <span style="color: #808080;">///</span><span style="color: #008000;"> Http请求参考类
    </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;/summary&gt;</span>
    <span style="color: #0000ff;">public</span> <span style="color: #0000ff;">class</span><span style="color: #000000;"> HttpItem
    {
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;summary&gt;</span>
        <span style="color: #808080;">///</span><span style="color: #008000;"> 请求URL必须填写
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;/summary&gt;</span>
        <span style="color: #0000ff;">public</span> <span style="color: #0000ff;">string</span> URL { <span style="color: #0000ff;">get</span>; <span style="color: #0000ff;">set</span><span style="color: #000000;">; }
        </span><span style="color: #0000ff;">string</span> _Method = <span style="color: #800000;">"</span><span style="color: #800000;">GET</span><span style="color: #800000;">"</span><span style="color: #000000;">;
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;summary&gt;</span>
        <span style="color: #808080;">///</span><span style="color: #008000;"> 请求方式默认为GET方式,当为POST方式时必须设置Postdata的值
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;/summary&gt;</span>
        <span style="color: #0000ff;">public</span> <span style="color: #0000ff;">string</span><span style="color: #000000;"> Method
        {
            </span><span style="color: #0000ff;">get</span> { <span style="color: #0000ff;">return</span><span style="color: #000000;"> _Method; }
            </span><span style="color: #0000ff;">set</span> { _Method =<span style="color: #000000;"> value; }
        }
        </span><span style="color: #0000ff;">int</span> _Timeout = <span style="color: #800080;">100000</span><span style="color: #000000;">;
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;summary&gt;</span>
        <span style="color: #808080;">///</span><span style="color: #008000;"> 默认请求超时时间
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;/summary&gt;</span>
        <span style="color: #0000ff;">public</span> <span style="color: #0000ff;">int</span><span style="color: #000000;"> Timeout
        {
            </span><span style="color: #0000ff;">get</span> { <span style="color: #0000ff;">return</span><span style="color: #000000;"> _Timeout; }
            </span><span style="color: #0000ff;">set</span> { _Timeout =<span style="color: #000000;"> value; }
        }
        </span><span style="color: #0000ff;">int</span> _ReadWriteTimeout = <span style="color: #800080;">30000</span><span style="color: #000000;">;
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;summary&gt;</span>
        <span style="color: #808080;">///</span><span style="color: #008000;"> 默认写入Post数据超时间
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;/summary&gt;</span>
        <span style="color: #0000ff;">public</span> <span style="color: #0000ff;">int</span><span style="color: #000000;"> ReadWriteTimeout
        {
            </span><span style="color: #0000ff;">get</span> { <span style="color: #0000ff;">return</span><span style="color: #000000;"> _ReadWriteTimeout; }
            </span><span style="color: #0000ff;">set</span> { _ReadWriteTimeout =<span style="color: #000000;"> value; }
        }
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;summary&gt;</span>
        <span style="color: #808080;">///</span><span style="color: #008000;"> 设置Host的标头信息
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;/summary&gt;</span>
        <span style="color: #0000ff;">public</span> <span style="color: #0000ff;">string</span> Host { <span style="color: #0000ff;">get</span>; <span style="color: #0000ff;">set</span><span style="color: #000000;">; }
        Boolean _KeepAlive </span>= <span style="color: #0000ff;">true</span><span style="color: #000000;">;
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;summary&gt;</span>
        <span style="color: #808080;">///</span><span style="color: #008000;">  获取或设置一个值，该值指示是否与 Internet 资源建立持久性连接默认为true。
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;/summary&gt;</span>
        <span style="color: #0000ff;">public</span><span style="color: #000000;"> Boolean KeepAlive
        {
            </span><span style="color: #0000ff;">get</span> { <span style="color: #0000ff;">return</span><span style="color: #000000;"> _KeepAlive; }
            </span><span style="color: #0000ff;">set</span> { _KeepAlive =<span style="color: #000000;"> value; }
        }
        </span><span style="color: #0000ff;">string</span> _Accept = <span style="color: #800000;">"</span><span style="color: #800000;">text/html, application/xhtml+xml, */*</span><span style="color: #800000;">"</span><span style="color: #000000;">;
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;summary&gt;</span>
        <span style="color: #808080;">///</span><span style="color: #008000;"> 请求标头值 默认为text/html, application/xhtml+xml, */*
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;/summary&gt;</span>
        <span style="color: #0000ff;">public</span> <span style="color: #0000ff;">string</span><span style="color: #000000;"> Accept
        {
            </span><span style="color: #0000ff;">get</span> { <span style="color: #0000ff;">return</span><span style="color: #000000;"> _Accept; }
            </span><span style="color: #0000ff;">set</span> { _Accept =<span style="color: #000000;"> value; }
        }
        </span><span style="color: #0000ff;">string</span> _ContentType = <span style="color: #800000;">"</span><span style="color: #800000;">text/html</span><span style="color: #800000;">"</span><span style="color: #000000;">;
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;summary&gt;</span>
        <span style="color: #808080;">///</span><span style="color: #008000;"> 请求返回类型默认 text/html
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;/summary&gt;</span>
        <span style="color: #0000ff;">public</span> <span style="color: #0000ff;">string</span><span style="color: #000000;"> ContentType
        {
            </span><span style="color: #0000ff;">get</span> { <span style="color: #0000ff;">return</span><span style="color: #000000;"> _ContentType; }
            </span><span style="color: #0000ff;">set</span> { _ContentType =<span style="color: #000000;"> value; }
        }
        </span><span style="color: #0000ff;">string</span> _UserAgent = <span style="color: #800000;">"</span><span style="color: #800000;">Mozilla/5.0 (compatible; MSIE 9.0; Windows NT 6.1; Trident/5.0)</span><span style="color: #800000;">"</span><span style="color: #000000;">;
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;summary&gt;</span>
        <span style="color: #808080;">///</span><span style="color: #008000;"> 客户端访问信息默认Mozilla/5.0 (compatible; MSIE 9.0; Windows NT 6.1; Trident/5.0)
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;/summary&gt;</span>
        <span style="color: #0000ff;">public</span> <span style="color: #0000ff;">string</span><span style="color: #000000;"> UserAgent
        {
            </span><span style="color: #0000ff;">get</span> { <span style="color: #0000ff;">return</span><span style="color: #000000;"> _UserAgent; }
            </span><span style="color: #0000ff;">set</span> { _UserAgent =<span style="color: #000000;"> value; }
        }
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;summary&gt;</span>
        <span style="color: #808080;">///</span><span style="color: #008000;"> 返回数据编码默认为NUll,可以自动识别,一般为utf-8,gbk,gb2312
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;/summary&gt;</span>
        <span style="color: #0000ff;">public</span> Encoding Encoding { <span style="color: #0000ff;">get</span>; <span style="color: #0000ff;">set</span><span style="color: #000000;">; }
        </span><span style="color: #0000ff;">private</span> PostDataType _PostDataType =<span style="color: #000000;"> PostDataType.String;
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;summary&gt;</span>
        <span style="color: #808080;">///</span><span style="color: #008000;"> Post的数据类型
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;/summary&gt;</span>
        <span style="color: #0000ff;">public</span><span style="color: #000000;"> PostDataType PostDataType
        {
            </span><span style="color: #0000ff;">get</span> { <span style="color: #0000ff;">return</span><span style="color: #000000;"> _PostDataType; }
            </span><span style="color: #0000ff;">set</span> { _PostDataType =<span style="color: #000000;"> value; }
        }
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;summary&gt;</span>
        <span style="color: #808080;">///</span><span style="color: #008000;"> Post请求时要发送的字符串Post数据
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;/summary&gt;</span>
        <span style="color: #0000ff;">public</span> <span style="color: #0000ff;">string</span> Postdata { <span style="color: #0000ff;">get</span>; <span style="color: #0000ff;">set</span><span style="color: #000000;">; }
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;summary&gt;</span>
        <span style="color: #808080;">///</span><span style="color: #008000;"> Post请求时要发送的Byte类型的Post数据
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;/summary&gt;</span>
        <span style="color: #0000ff;">public</span> <span style="color: #0000ff;">byte</span>[] PostdataByte { <span style="color: #0000ff;">get</span>; <span style="color: #0000ff;">set</span><span style="color: #000000;">; }
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;summary&gt;</span>
        <span style="color: #808080;">///</span><span style="color: #008000;"> Cookie对象集合
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;/summary&gt;</span>
        <span style="color: #0000ff;">public</span> CookieCollection CookieCollection { <span style="color: #0000ff;">get</span>; <span style="color: #0000ff;">set</span><span style="color: #000000;">; }
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;summary&gt;</span>
        <span style="color: #808080;">///</span><span style="color: #008000;"> 请求时的Cookie
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;/summary&gt;</span>
        <span style="color: #0000ff;">public</span> <span style="color: #0000ff;">string</span> Cookie { <span style="color: #0000ff;">get</span>; <span style="color: #0000ff;">set</span><span style="color: #000000;">; }
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;summary&gt;</span>
        <span style="color: #808080;">///</span><span style="color: #008000;"> 来源地址，上次访问地址
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;/summary&gt;</span>
        <span style="color: #0000ff;">public</span> <span style="color: #0000ff;">string</span> Referer { <span style="color: #0000ff;">get</span>; <span style="color: #0000ff;">set</span><span style="color: #000000;">; }
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;summary&gt;</span>
        <span style="color: #808080;">///</span><span style="color: #008000;"> 证书绝对路径
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;/summary&gt;</span>
        <span style="color: #0000ff;">public</span> <span style="color: #0000ff;">string</span> CerPath { <span style="color: #0000ff;">get</span>; <span style="color: #0000ff;">set</span><span style="color: #000000;">; }
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;summary&gt;</span>
        <span style="color: #808080;">///</span><span style="color: #008000;"> 设置代理对象，不想使用IE默认配置就设置为Null，而且不要设置ProxyIp
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;/summary&gt;</span>
        <span style="color: #0000ff;">public</span> WebProxy WebProxy { <span style="color: #0000ff;">get</span>; <span style="color: #0000ff;">set</span><span style="color: #000000;">; }
        </span><span style="color: #0000ff;">private</span> Boolean isToLower = <span style="color: #0000ff;">false</span><span style="color: #000000;">;
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;summary&gt;</span>
        <span style="color: #808080;">///</span><span style="color: #008000;"> 是否设置为全文小写，默认为不转化
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;/summary&gt;</span>
        <span style="color: #0000ff;">public</span><span style="color: #000000;"> Boolean IsToLower
        {
            </span><span style="color: #0000ff;">get</span> { <span style="color: #0000ff;">return</span><span style="color: #000000;"> isToLower; }
            </span><span style="color: #0000ff;">set</span> { isToLower =<span style="color: #000000;"> value; }
        }
        </span><span style="color: #0000ff;">private</span> Boolean allowautoredirect = <span style="color: #0000ff;">false</span><span style="color: #000000;">;
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;summary&gt;</span>
        <span style="color: #808080;">///</span><span style="color: #008000;"> 支持跳转页面，查询结果将是跳转后的页面，默认是不跳转
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;/summary&gt;</span>
        <span style="color: #0000ff;">public</span><span style="color: #000000;"> Boolean Allowautoredirect
        {
            </span><span style="color: #0000ff;">get</span> { <span style="color: #0000ff;">return</span><span style="color: #000000;"> allowautoredirect; }
            </span><span style="color: #0000ff;">set</span> { allowautoredirect =<span style="color: #000000;"> value; }
        }
        </span><span style="color: #0000ff;">private</span> <span style="color: #0000ff;">int</span> connectionlimit = <span style="color: #800080;">1024</span><span style="color: #000000;">;
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;summary&gt;</span>
        <span style="color: #808080;">///</span><span style="color: #008000;"> 最大连接数
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;/summary&gt;</span>
        <span style="color: #0000ff;">public</span> <span style="color: #0000ff;">int</span><span style="color: #000000;"> Connectionlimit
        {
            </span><span style="color: #0000ff;">get</span> { <span style="color: #0000ff;">return</span><span style="color: #000000;"> connectionlimit; }
            </span><span style="color: #0000ff;">set</span> { connectionlimit =<span style="color: #000000;"> value; }
        }
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;summary&gt;</span>
        <span style="color: #808080;">///</span><span style="color: #008000;"> 代理Proxy 服务器用户名
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;/summary&gt;</span>
        <span style="color: #0000ff;">public</span> <span style="color: #0000ff;">string</span> ProxyUserName { <span style="color: #0000ff;">get</span>; <span style="color: #0000ff;">set</span><span style="color: #000000;">; }
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;summary&gt;</span>
        <span style="color: #808080;">///</span><span style="color: #008000;"> 代理 服务器密码
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;/summary&gt;</span>
        <span style="color: #0000ff;">public</span> <span style="color: #0000ff;">string</span> ProxyPwd { <span style="color: #0000ff;">get</span>; <span style="color: #0000ff;">set</span><span style="color: #000000;">; }
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;summary&gt;</span>
        <span style="color: #808080;">///</span><span style="color: #008000;"> 代理 服务IP,如果要使用IE代理就设置为ieproxy
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;/summary&gt;</span>
        <span style="color: #0000ff;">public</span> <span style="color: #0000ff;">string</span> ProxyIp { <span style="color: #0000ff;">get</span>; <span style="color: #0000ff;">set</span><span style="color: #000000;">; }
        </span><span style="color: #0000ff;">private</span> ResultType resulttype =<span style="color: #000000;"> ResultType.String;
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;summary&gt;</span>
        <span style="color: #808080;">///</span><span style="color: #008000;"> 设置返回类型String和Byte
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;/summary&gt;</span>
        <span style="color: #0000ff;">public</span><span style="color: #000000;"> ResultType ResultType
        {
            </span><span style="color: #0000ff;">get</span> { <span style="color: #0000ff;">return</span><span style="color: #000000;"> resulttype; }
            </span><span style="color: #0000ff;">set</span> { resulttype =<span style="color: #000000;"> value; }
        }
        </span><span style="color: #0000ff;">private</span> WebHeaderCollection header = <span style="color: #0000ff;">new</span><span style="color: #000000;"> WebHeaderCollection();
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;summary&gt;</span>
        <span style="color: #808080;">///</span><span style="color: #008000;"> header对象
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;/summary&gt;</span>
        <span style="color: #0000ff;">public</span><span style="color: #000000;"> WebHeaderCollection Header
        {
            </span><span style="color: #0000ff;">get</span> { <span style="color: #0000ff;">return</span><span style="color: #000000;"> header; }
            </span><span style="color: #0000ff;">set</span> { header =<span style="color: #000000;"> value; }
        }
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;summary&gt;</span>
        <span style="color: #008000;">//</span><span style="color: #008000;">     获取或设置用于请求的 HTTP 版本。返回结果:用于请求的 HTTP 版本。默认为 System.Net.HttpVersion.Version11。</span>
        <span style="color: #808080;">///</span> <span style="color: #808080;">&lt;/summary&gt;</span>
        <span style="color: #0000ff;">public</span> Version ProtocolVersion { <span style="color: #0000ff;">get</span>; <span style="color: #0000ff;">set</span><span style="color: #000000;">; }
        </span><span style="color: #0000ff;">private</span> Boolean _expect100continue = <span style="color: #0000ff;">false</span><span style="color: #000000;">;
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;summary&gt;</span>
        <span style="color: #808080;">///</span><span style="color: #008000;">  获取或设置一个 System.Boolean 值，该值确定是否使用 100-Continue 行为。如果 POST 请求需要 100-Continue 响应，则为 true；否则为 false。默认值为 true。
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;/summary&gt;</span>
        <span style="color: #0000ff;">public</span><span style="color: #000000;"> Boolean Expect100Continue
        {
            </span><span style="color: #0000ff;">get</span> { <span style="color: #0000ff;">return</span><span style="color: #000000;"> _expect100continue; }
            </span><span style="color: #0000ff;">set</span> { _expect100continue =<span style="color: #000000;"> value; }
        }
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;summary&gt;</span>
        <span style="color: #808080;">///</span><span style="color: #008000;"> 设置509证书集合
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;/summary&gt;</span>
        <span style="color: #0000ff;">public</span> X509CertificateCollection ClentCertificates { <span style="color: #0000ff;">get</span>; <span style="color: #0000ff;">set</span><span style="color: #000000;">; }
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;summary&gt;</span>
        <span style="color: #808080;">///</span><span style="color: #008000;"> 设置或获取Post参数编码,默认的为Default编码
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;/summary&gt;</span>
        <span style="color: #0000ff;">public</span> Encoding PostEncoding { <span style="color: #0000ff;">get</span>; <span style="color: #0000ff;">set</span><span style="color: #000000;">; }
        </span><span style="color: #0000ff;">private</span> ResultCookieType _ResultCookieType =<span style="color: #000000;"> ResultCookieType.String;
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;summary&gt;</span>
        <span style="color: #808080;">///</span><span style="color: #008000;"> Cookie返回类型,默认的是只返回字符串类型
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;/summary&gt;</span>
        <span style="color: #0000ff;">public</span><span style="color: #000000;"> ResultCookieType ResultCookieType
        {
            </span><span style="color: #0000ff;">get</span> { <span style="color: #0000ff;">return</span><span style="color: #000000;"> _ResultCookieType; }
            </span><span style="color: #0000ff;">set</span> { _ResultCookieType =<span style="color: #000000;"> value; }
        }
        </span><span style="color: #0000ff;">private</span> ICredentials _ICredentials =<span style="color: #000000;"> CredentialCache.DefaultCredentials;
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;summary&gt;</span>
        <span style="color: #808080;">///</span><span style="color: #008000;"> 获取或设置请求的身份验证信息。
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;/summary&gt;</span>
        <span style="color: #0000ff;">public</span><span style="color: #000000;"> ICredentials ICredentials
        {
            </span><span style="color: #0000ff;">get</span> { <span style="color: #0000ff;">return</span><span style="color: #000000;"> _ICredentials; }
            </span><span style="color: #0000ff;">set</span> { _ICredentials =<span style="color: #000000;"> value; }
        }
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;summary&gt;</span>
        <span style="color: #808080;">///</span><span style="color: #008000;"> 设置请求将跟随的重定向的最大数目
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;/summary&gt;</span>
        <span style="color: #0000ff;">public</span> <span style="color: #0000ff;">int</span> MaximumAutomaticRedirections { <span style="color: #0000ff;">get</span>; <span style="color: #0000ff;">set</span><span style="color: #000000;">; }
        </span><span style="color: #0000ff;">private</span> DateTime? _IfModifiedSince = <span style="color: #0000ff;">null</span><span style="color: #000000;">;
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;summary&gt;</span>
        <span style="color: #808080;">///</span><span style="color: #008000;"> 获取和设置IfModifiedSince，默认为当前日期和时间
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;/summary&gt;</span>
        <span style="color: #0000ff;">public</span> DateTime?<span style="color: #000000;"> IfModifiedSince
        {
            </span><span style="color: #0000ff;">get</span> { <span style="color: #0000ff;">return</span><span style="color: #000000;"> _IfModifiedSince; }
            </span><span style="color: #0000ff;">set</span> { _IfModifiedSince =<span style="color: #000000;"> value; }
        }
        </span><span style="color: #0000ff;">#region</span> ip-port
        <span style="color: #0000ff;">private</span> IPEndPoint _IPEndPoint = <span style="color: #0000ff;">null</span><span style="color: #000000;">;
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;summary&gt;</span>
        <span style="color: #808080;">///</span><span style="color: #008000;"> 设置本地的出口ip和端口
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;/summary&gt;</span><span style="color: #008000;">]
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;example&gt;</span>
        <span style="color: #808080;">///</span><span style="color: #008000;">item.IPEndPoint = new IPEndPoint(IPAddress.Parse("192.168.1.1"),80);
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;/example&gt;</span>
        <span style="color: #0000ff;">public</span><span style="color: #000000;"> IPEndPoint IPEndPoint
        {
            </span><span style="color: #0000ff;">get</span> { <span style="color: #0000ff;">return</span><span style="color: #000000;"> _IPEndPoint; }
            </span><span style="color: #0000ff;">set</span> { _IPEndPoint =<span style="color: #000000;"> value; }
        }
        </span><span style="color: #0000ff;">#endregion</span>

        <span style="color: #0000ff;">private</span> <span style="color: #0000ff;">bool</span> _isReset = <span style="color: #0000ff;">false</span><span style="color: #000000;">;
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;summary&gt;</span>
        <span style="color: #808080;">///</span><span style="color: #008000;"> 是否重置request,response的值，默认不重置，当设置为True时request,response将被设置为Null
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;/summary&gt;</span>
        <span style="color: #0000ff;">public</span> <span style="color: #0000ff;">bool</span><span style="color: #000000;"> IsReset
        {
            </span><span style="color: #0000ff;">get</span> { <span style="color: #0000ff;">return</span><span style="color: #000000;"> _isReset; }
            </span><span style="color: #0000ff;">set</span> { _isReset =<span style="color: #000000;"> value; }
        }
    }
    </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;summary&gt;</span>
    <span style="color: #808080;">///</span><span style="color: #008000;"> Http返回参数类
    </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;/summary&gt;</span>
    <span style="color: #0000ff;">public</span> <span style="color: #0000ff;">class</span><span style="color: #000000;"> HttpResult
    {
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;summary&gt;</span>
        <span style="color: #808080;">///</span><span style="color: #008000;"> Http请求返回的Cookie
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;/summary&gt;</span>
        <span style="color: #0000ff;">public</span> <span style="color: #0000ff;">string</span> Cookie { <span style="color: #0000ff;">get</span>; <span style="color: #0000ff;">set</span><span style="color: #000000;">; }
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;summary&gt;</span>
        <span style="color: #808080;">///</span><span style="color: #008000;"> Cookie对象集合
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;/summary&gt;</span>
        <span style="color: #0000ff;">public</span> CookieCollection CookieCollection { <span style="color: #0000ff;">get</span>; <span style="color: #0000ff;">set</span><span style="color: #000000;">; }
        </span><span style="color: #0000ff;">private</span> <span style="color: #0000ff;">string</span> _html = <span style="color: #0000ff;">string</span><span style="color: #000000;">.Empty;
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;summary&gt;</span>
        <span style="color: #808080;">///</span><span style="color: #008000;"> 返回的String类型数据 只有ResultType.String时才返回数据，其它情况为空
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;/summary&gt;</span>
        <span style="color: #0000ff;">public</span> <span style="color: #0000ff;">string</span><span style="color: #000000;"> Html
        {
            </span><span style="color: #0000ff;">get</span> { <span style="color: #0000ff;">return</span><span style="color: #000000;"> _html; }
            </span><span style="color: #0000ff;">set</span> { _html =<span style="color: #000000;"> value; }
        }
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;summary&gt;</span>
        <span style="color: #808080;">///</span><span style="color: #008000;"> 返回的Byte数组 只有ResultType.Byte时才返回数据，其它情况为空
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;/summary&gt;</span>
        <span style="color: #0000ff;">public</span> <span style="color: #0000ff;">byte</span>[] ResultByte { <span style="color: #0000ff;">get</span>; <span style="color: #0000ff;">set</span><span style="color: #000000;">; }
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;summary&gt;</span>
        <span style="color: #808080;">///</span><span style="color: #008000;"> header对象
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;/summary&gt;</span>
        <span style="color: #0000ff;">public</span> WebHeaderCollection Header { <span style="color: #0000ff;">get</span>; <span style="color: #0000ff;">set</span><span style="color: #000000;">; }
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;summary&gt;</span>
        <span style="color: #808080;">///</span><span style="color: #008000;"> 返回状态说明
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;/summary&gt;</span>
        <span style="color: #0000ff;">public</span> <span style="color: #0000ff;">string</span> StatusDescription { <span style="color: #0000ff;">get</span>; <span style="color: #0000ff;">set</span><span style="color: #000000;">; }
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;summary&gt;</span>
        <span style="color: #808080;">///</span><span style="color: #008000;"> 返回状态码,默认为OK
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;/summary&gt;</span>
        <span style="color: #0000ff;">public</span> HttpStatusCode StatusCode { <span style="color: #0000ff;">get</span>; <span style="color: #0000ff;">set</span><span style="color: #000000;">; }
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;summary&gt;</span>
        <span style="color: #808080;">///</span><span style="color: #008000;"> 最后访问的URl
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;/summary&gt;</span>
        <span style="color: #0000ff;">public</span> <span style="color: #0000ff;">string</span> ResponseUri { <span style="color: #0000ff;">get</span>; <span style="color: #0000ff;">set</span><span style="color: #000000;">; }
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;summary&gt;</span>
        <span style="color: #808080;">///</span><span style="color: #008000;"> 获取重定向的URl
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;/summary&gt;</span>
        <span style="color: #0000ff;">public</span> <span style="color: #0000ff;">string</span><span style="color: #000000;"> RedirectUrl
        {
            </span><span style="color: #0000ff;">get</span><span style="color: #000000;">
            {
                </span><span style="color: #0000ff;">try</span><span style="color: #000000;">
                {
                    </span><span style="color: #0000ff;">if</span> (Header != <span style="color: #0000ff;">null</span> &amp;&amp; Header.Count &gt; <span style="color: #800080;">0</span><span style="color: #000000;">)
                    {
                        </span><span style="color: #0000ff;">if</span> (Header.AllKeys.Any(k =&gt; k.ToLower().Contains(<span style="color: #800000;">"</span><span style="color: #800000;">location</span><span style="color: #800000;">"</span><span style="color: #000000;">)))
                        {
                            </span><span style="color: #0000ff;">string</span> baseurl = Header[<span style="color: #800000;">"</span><span style="color: #800000;">location</span><span style="color: #800000;">"</span><span style="color: #000000;">].ToString().Trim();
                            </span><span style="color: #0000ff;">string</span> locationurl =<span style="color: #000000;"> baseurl.ToLower();
                            </span><span style="color: #0000ff;">if</span> (!<span style="color: #0000ff;">string</span><span style="color: #000000;">.IsNullOrWhiteSpace(locationurl))
                            {
                                </span><span style="color: #0000ff;">bool</span> b = locationurl.StartsWith(<span style="color: #800000;">"</span><span style="color: #800000;">http://</span><span style="color: #800000;">"</span>) || locationurl.StartsWith(<span style="color: #800000;">"</span><span style="color: #800000;">https://</span><span style="color: #800000;">"</span><span style="color: #000000;">);
                                </span><span style="color: #0000ff;">if</span> (!<span style="color: #000000;">b)
                                {
                                    baseurl </span>= <span style="color: #0000ff;">new</span> Uri(<span style="color: #0000ff;">new</span><span style="color: #000000;"> Uri(ResponseUri), baseurl).AbsoluteUri;
                                }
                            }
                            </span><span style="color: #0000ff;">return</span><span style="color: #000000;"> baseurl;
                        }
                    }
                }
                </span><span style="color: #0000ff;">catch</span><span style="color: #000000;"> { }
                </span><span style="color: #0000ff;">return</span> <span style="color: #0000ff;">string</span><span style="color: #000000;">.Empty;
            }
        }
    }
    </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;summary&gt;</span>
    <span style="color: #808080;">///</span><span style="color: #008000;"> 返回类型
    </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;/summary&gt;</span>
    <span style="color: #0000ff;">public</span> <span style="color: #0000ff;">enum</span><span style="color: #000000;"> ResultType
    {
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;summary&gt;</span>
        <span style="color: #808080;">///</span><span style="color: #008000;"> 表示只返回字符串 只有Html有数据
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;/summary&gt;</span>
<span style="color: #000000;">        String,
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;summary&gt;</span>
        <span style="color: #808080;">///</span><span style="color: #008000;"> 表示返回字符串和字节流 ResultByte和Html都有数据返回
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;/summary&gt;</span>
<span style="color: #000000;">        Byte
    }
    </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;summary&gt;</span>
    <span style="color: #808080;">///</span><span style="color: #008000;"> Post的数据格式默认为string
    </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;/summary&gt;</span>
    <span style="color: #0000ff;">public</span> <span style="color: #0000ff;">enum</span><span style="color: #000000;"> PostDataType
    {
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;summary&gt;</span>
        <span style="color: #808080;">///</span><span style="color: #008000;"> 字符串类型，这时编码Encoding可不设置
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;/summary&gt;</span>
<span style="color: #000000;">        String,
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;summary&gt;</span>
        <span style="color: #808080;">///</span><span style="color: #008000;"> Byte类型，需要设置PostdataByte参数的值编码Encoding可设置为空
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;/summary&gt;</span>
<span style="color: #000000;">        Byte,
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;summary&gt;</span>
        <span style="color: #808080;">///</span><span style="color: #008000;"> 传文件，Postdata必须设置为文件的绝对路径，必须设置Encoding的值
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;/summary&gt;</span>
<span style="color: #000000;">        FilePath
    }
    </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;summary&gt;</span>
    <span style="color: #808080;">///</span><span style="color: #008000;"> Cookie返回类型
    </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;/summary&gt;</span>
    <span style="color: #0000ff;">public</span> <span style="color: #0000ff;">enum</span><span style="color: #000000;"> ResultCookieType
    {
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;summary&gt;</span>
        <span style="color: #808080;">///</span><span style="color: #008000;"> 只返回字符串类型的Cookie
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;/summary&gt;</span>
<span style="color: #000000;">        String,
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;summary&gt;</span>
        <span style="color: #808080;">///</span><span style="color: #008000;"> CookieCollection格式的Cookie集合同时也返回String类型的cookie
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;/summary&gt;</span>
<span style="color: #000000;">        CookieCollection
    }
    </span><span style="color: #0000ff;">#endregion</span><span style="color: #000000;">
}</span></pre>
</div>
<p>&nbsp;</p>