<p><span style="font-family: arial, helvetica, sans-serif;">　　本次记录只考虑单纯的技术实现，为了掌握设置敏感Header字段隐</span><span style="font-family: arial, helvetica, sans-serif;">藏的方法，对于是否有必要设置，此处不做讨论。</span></p>
<p><span style="font-family: arial, helvetica, sans-serif;">背景：对于Asp.Net MVC Web项目而言，若不做任何特殊设置，项目发布之后正常运行，但会打开浏览器开发者工具调试模式，点击Network选项，随便查看一个请求，会发现如下信息。</span></p>
<p style="text-align: center;"><span style="font-family: arial, helvetica, sans-serif;"><img src="https://images2018.cnblogs.com/blog/1159701/201803/1159701-20180308205917301-906288881.png" alt="" /></span></p>
<p style="text-align: left;"><span style="font-family: arial, helvetica, sans-serif;">　　就项目安全性而言，是不应该直接项目服务器版本、开发环境版本、框架信息等敏感信息返回到客户端的。那么，敏感参数包括那些？如何做到不将这部分信息展示在客户端呢？</span></p>
<p style="text-align: left;"><span style="font-family: arial, helvetica, sans-serif;">敏感Header字段：</span></p>
<p>&nbsp;</p>