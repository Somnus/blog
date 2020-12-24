<div class="cnblogs_code">
<pre><span style="color: #0000ff;">using</span><span style="color: #000000;"> System;
</span><span style="color: #0000ff;">using</span><span style="color: #000000;"> System.Collections.Generic;
</span><span style="color: #0000ff;">using</span><span style="color: #000000;"> System.Linq;
</span><span style="color: #0000ff;">using</span><span style="color: #000000;"> System.Text;
</span><span style="color: #0000ff;">using</span><span style="color: #000000;"> System.Net;
</span><span style="color: #0000ff;">using</span><span style="color: #000000;"> System.Net.Sockets;
</span><span style="color: #0000ff;">using</span><span style="color: #000000;"> System.Net.NetworkInformation;
</span><span style="color: #0000ff;">using</span><span style="color: #000000;"> System.Collections;
</span><span style="color: #0000ff;">using</span><span style="color: #000000;"> System.Threading;

</span><span style="color: #0000ff;">namespace</span><span style="color: #000000;"> IServer {
    </span><span style="color: #0000ff;">class</span><span style="color: #000000;"> Program 
    {
        </span><span style="color: #0000ff;">static</span> <span style="color: #0000ff;">void</span> Main(<span style="color: #0000ff;">string</span><span style="color: #000000;">[] args) 
        {
            </span><span style="color: #0000ff;">try</span><span style="color: #000000;">
            {
                </span><span style="color: #0000ff;">var</span> host = System.Net.Dns.GetHostEntry(System.Net.Dns.GetHostName()).AddressList[<span style="color: #800080;">2</span><span style="color: #000000;">];
                </span><span style="color: #0000ff;">var</span> port = <span style="color: #800080;">5001</span><span style="color: #000000;">;
                </span><span style="color: #0000ff;">var</span> IPEndPoint = <span style="color: #0000ff;">new</span><span style="color: #000000;"> IPEndPoint(host, port);
                </span><span style="color: #0000ff;">var</span> Socket = <span style="color: #0000ff;">new</span><span style="color: #000000;"> Socket(AddressFamily.InterNetwork, SocketType.Stream, ProtocolType.Tcp);
                Socket.Bind(IPEndPoint);
                Socket.Listen(</span><span style="color: #800080;">10</span><span style="color: #000000;">);
                Console.WriteLine(</span><span style="color: #800000;">"</span><span style="color: #800000;">等待客户端连接</span><span style="color: #800000;">"</span><span style="color: #000000;">);
                </span><span style="color: #0000ff;">var</span> temp =<span style="color: #000000;"> Socket.Accept();
                Console.WriteLine(</span><span style="color: #800000;">"</span><span style="color: #800000;">客户端已连接，地址为{0}</span><span style="color: #800000;">"</span><span style="color: #000000;">,temp.RemoteEndPoint);

                </span><span style="color: #0000ff;">var</span> t1 = <span style="color: #0000ff;">new</span> Thread(<span style="color: #0000ff;">new</span><span style="color: #000000;"> ParameterizedThreadStart(Send));
                </span><span style="color: #0000ff;">var</span> t2 = <span style="color: #0000ff;">new</span> Thread(<span style="color: #0000ff;">new</span><span style="color: #000000;"> ParameterizedThreadStart(Recive));
                t1.Start(temp);
                t2.Start(temp);
            }
            </span><span style="color: #0000ff;">catch</span><span style="color: #000000;"> (ArgumentNullException e)
            {
                Console.WriteLine(</span><span style="color: #800000;">"</span><span style="color: #800000;">ArgumentNullException: {0}</span><span style="color: #800000;">"</span><span style="color: #000000;">, e);
            }
            </span><span style="color: #0000ff;">catch</span><span style="color: #000000;"> (SocketException e)
            {
                Console.WriteLine(</span><span style="color: #800000;">"</span><span style="color: #800000;">SocketException: {0}</span><span style="color: #800000;">"</span><span style="color: #000000;">, e);
            }
            Console.ReadLine();

　　        }

        </span><span style="color: #0000ff;">public</span> <span style="color: #0000ff;">static</span> <span style="color: #0000ff;">void</span> Send(<span style="color: #0000ff;">object</span><span style="color: #000000;"> temp)
        {
            </span><span style="color: #0000ff;">while</span> (<span style="color: #0000ff;">true</span><span style="color: #000000;">)
            {
                </span><span style="color: #0000ff;">string</span> sendStr =<span style="color: #000000;"> Console.ReadLine().ToString();
                </span><span style="color: #0000ff;">byte</span>[] bs =<span style="color: #000000;"> Encoding.Unicode.GetBytes(sendStr);
                ((Socket)temp).Send(bs, bs.Length, </span><span style="color: #800080;">0</span><span style="color: #000000;">);
            }
        }

        </span><span style="color: #0000ff;">public</span> <span style="color: #0000ff;">static</span> <span style="color: #0000ff;">void</span> Recive(<span style="color: #0000ff;">object</span><span style="color: #000000;"> temp)
        {
            </span><span style="color: #0000ff;">while</span> (<span style="color: #0000ff;">true</span><span style="color: #000000;">)
            {
                </span><span style="color: #0000ff;">string</span> recvStr = <span style="color: #800000;">""</span><span style="color: #000000;">;
                </span><span style="color: #0000ff;">byte</span>[] recvBytes = <span style="color: #0000ff;">new</span> <span style="color: #0000ff;">byte</span>[<span style="color: #800080;">1024</span><span style="color: #000000;">];
                </span><span style="color: #0000ff;">var</span> bytes = ((Socket)temp).Receive(recvBytes, recvBytes.Length, <span style="color: #800080;">0</span>);<span style="color: #008000;">//</span><span style="color: #008000;">从客户端接受信息</span>
                recvStr += Encoding.Unicode.GetString(recvBytes, <span style="color: #800080;">0</span><span style="color: #000000;">, bytes);
                Console.WriteLine(</span><span style="color: #800000;">"</span><span style="color: #800000;">Client:{0}</span><span style="color: #800000;">"</span>, recvStr);<span style="color: #008000;">//</span><span style="color: #008000;">把客户端传来的信息显示出来</span>
<span style="color: #000000;">            }
        }

        </span><span style="color: #0000ff;">public</span> <span style="color: #0000ff;">static</span> <span style="color: #0000ff;">void</span><span style="color: #000000;"> Closed(Socket Socket,Socket temp)
        {
            temp.Close();
            Socket.Close();
        }
    }
}</span></pre>
</div>
<p>&nbsp;</p>