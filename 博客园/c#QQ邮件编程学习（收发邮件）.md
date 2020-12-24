<p>本次c#实现邮件管理编程的目的是实现第三方邮件管理，邮箱基于QQ邮箱，发送邮件直接采用.NET自带的System.Net.Mail类，接收邮件采用第三方组件Lumisoft.Net。现将基本实现的接收邮件和发送邮件代码记录如下。</p>
<p>1.发送邮件：基于System.Net.Mail。</p>
<div class="cnblogs_code">
<pre><span style="color: #0000ff;">using</span> System.Net.Mail;</pre>
</div>
<div class="cnblogs_code">
<pre>  <span style="color: #0000ff;">public</span> <span style="color: #0000ff;">void</span> SendMail(<span style="color: #0000ff;">string</span> MailFrom, <span style="color: #0000ff;">string</span> MailTo, <span style="color: #0000ff;">string</span> MailPwd, <span style="color: #0000ff;">string</span> Mailtitle,<span style="color: #0000ff;">string</span> MailCon,<span style="color: #0000ff;">string</span><span style="color: #000000;"> attachMentUrl)
        {
            SmtpClient client </span>= <span style="color: #0000ff;">new</span> SmtpClient(<span style="color: #800000;">"</span><span style="color: #800000;">smtp.qq.com</span><span style="color: #800000;">"</span><span style="color: #000000;">);
            client.EnableSsl </span>= <span style="color: #0000ff;">true</span><span style="color: #000000;">;
            client.UseDefaultCredentials </span>= <span style="color: #0000ff;">false</span><span style="color: #000000;">;
            client.Credentials </span>= <span style="color: #0000ff;">new</span> System.Net.NetworkCredential(MailTo + <span style="color: #800000;">"</span><span style="color: #800000;">@qq.com</span><span style="color: #800000;">"</span><span style="color: #000000;">, MailPwd);

            MailAddress From </span>= <span style="color: #0000ff;">new</span> MailAddress(MailFrom + <span style="color: #800000;">"</span><span style="color: #800000;">@qq.com</span><span style="color: #800000;">"</span>, <span style="color: #800000;">"</span><span style="color: #800000;">fxy</span><span style="color: #800000;">"</span><span style="color: #000000;">, Encoding.UTF8);
            MailAddress To </span>= <span style="color: #0000ff;">new</span> MailAddress(MailTo + <span style="color: #800000;">"</span><span style="color: #800000;">@qq.com</span><span style="color: #800000;">"</span>, <span style="color: #800000;">""</span><span style="color: #000000;">, Encoding.UTF8);

            MailMessage myMessage </span>= <span style="color: #0000ff;">new</span><span style="color: #000000;"> MailMessage(From,To);
            myMessage.Body </span>=<span style="color: #000000;"> MailCon;
            myMessage.BodyEncoding </span>=<span style="color: #000000;"> Encoding.UTF8;
            myMessage.Subject </span>=<span style="color: #000000;"> Mailtitle;
            myMessage.SubjectEncoding </span>=<span style="color: #000000;"> Encoding.UTF8;
            myMessage.IsBodyHtml </span>= <span style="color: #0000ff;">true</span><span style="color: #000000;">;

            Attachment attachment </span>= <span style="color: #0000ff;">new</span><span style="color: #000000;"> Attachment(attachMentUrl);
            myMessage.Attachments.Add(attachment);

            </span><span style="color: #0000ff;">try</span><span style="color: #000000;">
            {
                client.Send(myMessage);
            }
            </span><span style="color: #0000ff;">catch</span><span style="color: #000000;"> (InvalidOperationException e)
            { }
            </span><span style="color: #0000ff;">catch</span><span style="color: #000000;"> (Exception e)
            { }
            </span><span style="color: #0000ff;">finally</span><span style="color: #000000;">
            {
                Console.ReadLine();
            }
        }</span></pre>
</div>
<p>2.接收邮件：基于Lumisoft.Net（</p>
<div class="cnblogs_code">
<pre><span style="color: #0000ff;">using</span><span style="color: #000000;"> LumiSoft.Net.POP3.Client;
</span><span style="color: #0000ff;">using</span> LumiSoft.Net.Mail;</pre>
</div>
<div class="cnblogs_code">
<pre> <span style="color: #0000ff;">public</span> <span style="color: #0000ff;">void</span><span style="color: #000000;"> GetEmails()
        {
            </span><span style="color: #0000ff;">using</span> (POP3_Client c = <span style="color: #0000ff;">new</span><span style="color: #000000;"> POP3_Client())
            {
                c.Connect(</span><span style="color: #800000;">"</span><span style="color: #800000;">pop.qq.com</span><span style="color: #800000;">"</span>, <span style="color: #800080;">995</span>, <span style="color: #0000ff;">true</span><span style="color: #000000;">);
                c.Login(</span><span style="color: #800000;">"</span><span style="color: #800000;">1300837979@qq.com</span><span style="color: #800000;">"</span>, <span style="color: #800000;">"</span><span style="color: #800000;">sjgqkszeqlcqgihc</span><span style="color: #800000;">"</span><span style="color: #000000;">);
                </span><span style="color: #0000ff;">if</span> (c.Messages.Count &gt; <span style="color: #800080;">0</span><span style="color: #000000;">)
                {
                    </span><span style="color: #0000ff;">for</span> (<span style="color: #0000ff;">var</span> i = <span style="color: #800080;">0</span>; i &lt; c.Messages.Count; i++<span style="color: #000000;">)
                    {
                        </span><span style="color: #0000ff;">var</span> t =<span style="color: #000000;"> Mail_Message.ParseFromByte(c.Messages[i].MessageToByte());
                        </span><span style="color: #0000ff;">var</span> <span style="color: #0000ff;">from</span> =<span style="color: #000000;"> t.From;
                        </span><span style="color: #0000ff;">var</span> to=<span style="color: #000000;">t.To;
                        </span><span style="color: #0000ff;">var</span> date =<span style="color: #000000;"> t.Date;
                        </span><span style="color: #0000ff;">var</span> subject =<span style="color: #000000;"> t.Subject;
                        </span><span style="color: #0000ff;">var</span> bodyText=<span style="color: #000000;">t.BodyText;
                    }
                    
                }
            }
        }</span></pre>
</div>
<p>&nbsp;</p>