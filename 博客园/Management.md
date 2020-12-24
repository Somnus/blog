<div class="cnblogs_code">
<pre><span style="color: #0000ff;">using</span><span style="color: #000000;"> System;
</span><span style="color: #0000ff;">using</span><span style="color: #000000;"> System.Collections.Generic;
</span><span style="color: #0000ff;">using</span><span style="color: #000000;"> System.Linq;
</span><span style="color: #0000ff;">using</span><span style="color: #000000;"> System.Text;
</span><span style="color: #0000ff;">using</span><span style="color: #000000;"> System.Threading.Tasks;
</span><span style="color: #0000ff;">using</span><span style="color: #000000;"> System.Management;

</span><span style="color: #0000ff;">namespace</span><span style="color: #000000;"> IPMACDemo
{
    </span><span style="color: #0000ff;">class</span><span style="color: #000000;"> Program
    {
        </span><span style="color: #0000ff;">static</span> <span style="color: #0000ff;">void</span> Main(<span style="color: #0000ff;">string</span><span style="color: #000000;">[] args)
        {

            Console.WriteLine(</span><span style="color: #800000;">"</span><span style="color: #800000;">计算机名称：{0}</span><span style="color: #800000;">"</span><span style="color: #000000;">,GetComputerName());
            Console.WriteLine(</span><span style="color: #800000;">"</span><span style="color: #800000;">操作系统：{0}</span><span style="color: #800000;">"</span><span style="color: #000000;">, GetSystemType());
            Console.WriteLine(</span><span style="color: #800000;">"</span><span style="color: #800000;">用户名：{0}</span><span style="color: #800000;">"</span><span style="color: #000000;">, GetUserName());
            Console.WriteLine(</span><span style="color: #800000;">"</span><span style="color: #800000;">计算机MAC地址：{0}</span><span style="color: #800000;">"</span><span style="color: #000000;">, GetMacAddress());
            Console.WriteLine(</span><span style="color: #800000;">"</span><span style="color: #800000;">计算机IP地址：{0}</span><span style="color: #800000;">"</span><span style="color: #000000;">, GetIpAddress());

            Console.WriteLine(</span><span style="color: #800000;">"</span><span style="color: #800000;">硬盘序列号:{0}</span><span style="color: #800000;">"</span><span style="color: #000000;">, GetDiskSerialNumber());
            Console.WriteLine(</span><span style="color: #800000;">"</span><span style="color: #800000;">硬盘大小：{0}</span><span style="color: #800000;">"</span>, Convert.ToDouble(GetSizeOfDisk()) /(<span style="color: #800080;">1024</span>*<span style="color: #800080;">1024</span>*<span style="color: #800080;">1024</span><span style="color: #000000;">));

            Console.WriteLine(</span><span style="color: #800000;">"</span><span style="color: #800000;">网卡地址：{0}</span><span style="color: #800000;">"</span><span style="color: #000000;">, GetMacAddress());
            Console.WriteLine(</span><span style="color: #800000;">"</span><span style="color: #800000;">显卡PNPDeviceID：{0}</span><span style="color: #800000;">"</span><span style="color: #000000;">, GetVideoPnpid());
            Console.WriteLine(</span><span style="color: #800000;">"</span><span style="color: #800000;">声卡PNPDeviceID :{0}</span><span style="color: #800000;">"</span><span style="color: #000000;">, GetSoundPnpid());

            Console.WriteLine(</span><span style="color: #800000;">"</span><span style="color: #800000;">主板制造商：{0}</span><span style="color: #800000;">"</span><span style="color: #000000;">, GetBoardManufacturer());
            Console.WriteLine(</span><span style="color: #800000;">"</span><span style="color: #800000;">主板编号：{0}</span><span style="color: #800000;">"</span><span style="color: #000000;">, GetBoardId());
            Console.WriteLine(</span><span style="color: #800000;">"</span><span style="color: #800000;">主板型号：{0}</span><span style="color: #800000;">"</span><span style="color: #000000;">, GetBoardType());


            Console.WriteLine(</span><span style="color: #800000;">"</span><span style="color: #800000;">CPU名称：{0}</span><span style="color: #800000;">"</span><span style="color: #000000;">, GetCpuName());
            Console.WriteLine(</span><span style="color: #800000;">"</span><span style="color: #800000;">CPU数量：{0}</span><span style="color: #800000;">"</span><span style="color: #000000;">, GetCpuCount());
            Console.WriteLine(</span><span style="color: #800000;">"</span><span style="color: #800000;">CPU编号：{0}</span><span style="color: #800000;">"</span><span style="color: #000000;">, GetCpuid());
            Console.WriteLine(</span><span style="color: #800000;">"</span><span style="color: #800000;">CPU版本信息：{0}</span><span style="color: #800000;">"</span><span style="color: #000000;">, GetCpuVersion());
            Console.WriteLine(</span><span style="color: #800000;">"</span><span style="color: #800000;">CPU制造商：{0}</span><span style="color: #800000;">"</span><span style="color: #000000;">, GetCpuManufacturer());

            Console.WriteLine(</span><span style="color: #800000;">"</span><span style="color: #800000;">物理内存：{0}</span><span style="color: #800000;">"</span>, Convert.ToDouble(GetPhysicalMemory()) / (<span style="color: #800080;">1024</span> * <span style="color: #800080;">1024</span> * <span style="color: #800080;">1024</span><span style="color: #000000;">));

            Console.ReadLine();

        }




        </span><span style="color: #0000ff;">#region</span> CPU
        <span style="color: #808080;">///</span> <span style="color: #808080;">&lt;summary&gt;</span>
        <span style="color: #808080;">///</span><span style="color: #008000;"> 获取CPU的频率 这里之所以使用string类型的数组，主要是因为cpu的多核
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;/summary&gt;</span>
        <span style="color: #808080;">///</span> <span style="color: #808080;">&lt;returns&gt;&lt;/returns&gt;</span>
        <span style="color: #0000ff;">public</span> <span style="color: #0000ff;">static</span> <span style="color: #0000ff;">string</span><span style="color: #000000;">[] GetCpuMHZ()
        {
            ManagementClass mc </span>= <span style="color: #0000ff;">new</span> ManagementClass(<span style="color: #800000;">"</span><span style="color: #800000;">Win32_Processor</span><span style="color: #800000;">"</span><span style="color: #000000;">);
            ManagementObjectCollection cpus </span>=<span style="color: #000000;"> mc.GetInstances();

            </span><span style="color: #0000ff;">string</span>[] mHz = <span style="color: #0000ff;">new</span> <span style="color: #0000ff;">string</span><span style="color: #000000;">[cpus.Count];
            </span><span style="color: #0000ff;">int</span> c = <span style="color: #800080;">0</span><span style="color: #000000;">;
            ManagementObjectSearcher mySearch </span>= <span style="color: #0000ff;">new</span> ManagementObjectSearcher(<span style="color: #800000;">"</span><span style="color: #800000;">select * from Win32_Processor</span><span style="color: #800000;">"</span><span style="color: #000000;">);
            </span><span style="color: #0000ff;">foreach</span> (ManagementObject mo <span style="color: #0000ff;">in</span><span style="color: #000000;"> mySearch.Get())
            {
                mHz[c] </span>= mo.Properties[<span style="color: #800000;">"</span><span style="color: #800000;">CurrentClockSpeed</span><span style="color: #800000;">"</span><span style="color: #000000;">].Value.ToString();
                c</span>++<span style="color: #000000;">;
            }
            mc.Dispose();
            mySearch.Dispose();
            </span><span style="color: #0000ff;">return</span><span style="color: #000000;"> mHz;
        }

        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;summary&gt;</span>
        <span style="color: #808080;">///</span><span style="color: #008000;"> 获取CPU的个数
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;/summary&gt;</span>
        <span style="color: #808080;">///</span> <span style="color: #808080;">&lt;returns&gt;&lt;/returns&gt;</span>
        <span style="color: #0000ff;">public</span> <span style="color: #0000ff;">static</span> <span style="color: #0000ff;">int</span><span style="color: #000000;"> GetCpuCount()
        {
            </span><span style="color: #0000ff;">try</span><span style="color: #000000;">
            {
                </span><span style="color: #0000ff;">using</span> (ManagementClass mCpu = <span style="color: #0000ff;">new</span> ManagementClass(<span style="color: #800000;">"</span><span style="color: #800000;">Win32_Processor</span><span style="color: #800000;">"</span><span style="color: #000000;">))
                {
                    ManagementObjectCollection cpus </span>=<span style="color: #000000;"> mCpu.GetInstances();
                    </span><span style="color: #0000ff;">return</span><span style="color: #000000;"> cpus.Count;
                }
            }
            </span><span style="color: #0000ff;">catch</span><span style="color: #000000;">
            {
            }
            </span><span style="color: #0000ff;">return</span> -<span style="color: #800080;">1</span><span style="color: #000000;">;
        }

        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;summary&gt;</span>  
        <span style="color: #808080;">///</span><span style="color: #008000;"> 获得CPU编号  
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;/summary&gt;</span>  
        <span style="color: #808080;">///</span> <span style="color: #808080;">&lt;returns&gt;&lt;/returns&gt;</span>  
        <span style="color: #0000ff;">public</span> <span style="color: #0000ff;">static</span> <span style="color: #0000ff;">string</span><span style="color: #000000;"> GetCpuid()
        {
            </span><span style="color: #0000ff;">var</span> cpuid = <span style="color: #0000ff;">string</span><span style="color: #000000;">.Empty;
            </span><span style="color: #0000ff;">var</span> mc = <span style="color: #0000ff;">new</span> ManagementClass(<span style="color: #800000;">"</span><span style="color: #800000;">Win32_Processor</span><span style="color: #800000;">"</span><span style="color: #000000;">);
            </span><span style="color: #0000ff;">var</span> moc =<span style="color: #000000;"> mc.GetInstances();
            </span><span style="color: #0000ff;">foreach</span> (<span style="color: #0000ff;">var</span> o <span style="color: #0000ff;">in</span><span style="color: #000000;"> moc)
            {
                </span><span style="color: #0000ff;">var</span> mo =<span style="color: #000000;"> (ManagementObject)o;
                cpuid </span>= mo.Properties[<span style="color: #800000;">"</span><span style="color: #800000;">ProcessorId</span><span style="color: #800000;">"</span><span style="color: #000000;">].Value.ToString();
            }
            </span><span style="color: #0000ff;">return</span><span style="color: #000000;"> cpuid;
        }

        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;summary&gt;</span>  
        <span style="color: #808080;">///</span><span style="color: #008000;"> CPU版本信息  
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;/summary&gt;</span>  
        <span style="color: #808080;">///</span> <span style="color: #808080;">&lt;returns&gt;&lt;/returns&gt;</span>  
        <span style="color: #0000ff;">public</span> <span style="color: #0000ff;">static</span> <span style="color: #0000ff;">string</span><span style="color: #000000;"> GetCpuVersion()
        {
            </span><span style="color: #0000ff;">var</span> st = <span style="color: #0000ff;">string</span><span style="color: #000000;">.Empty;
            </span><span style="color: #0000ff;">var</span> mos = <span style="color: #0000ff;">new</span> ManagementObjectSearcher(<span style="color: #800000;">"</span><span style="color: #800000;">Select * from Win32_Processor</span><span style="color: #800000;">"</span><span style="color: #000000;">);
            </span><span style="color: #0000ff;">foreach</span> (<span style="color: #0000ff;">var</span> o <span style="color: #0000ff;">in</span><span style="color: #000000;"> mos.Get())
            {
                </span><span style="color: #0000ff;">var</span> mo =<span style="color: #000000;"> (ManagementObject)o;
                st </span>= mo[<span style="color: #800000;">"</span><span style="color: #800000;">Version</span><span style="color: #800000;">"</span><span style="color: #000000;">].ToString();
            }
            </span><span style="color: #0000ff;">return</span><span style="color: #000000;"> st;
        }

        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;summary&gt;</span>  
        <span style="color: #808080;">///</span><span style="color: #008000;"> CPU名称信息  
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;/summary&gt;</span>  
        <span style="color: #808080;">///</span> <span style="color: #808080;">&lt;returns&gt;&lt;/returns&gt;</span>  
        <span style="color: #0000ff;">public</span> <span style="color: #0000ff;">static</span> <span style="color: #0000ff;">string</span><span style="color: #000000;"> GetCpuName()
        {
            </span><span style="color: #0000ff;">var</span> st = <span style="color: #0000ff;">string</span><span style="color: #000000;">.Empty;
            </span><span style="color: #0000ff;">var</span> driveId = <span style="color: #0000ff;">new</span> ManagementObjectSearcher(<span style="color: #800000;">"</span><span style="color: #800000;">Select * from Win32_Processor</span><span style="color: #800000;">"</span><span style="color: #000000;">);
            </span><span style="color: #0000ff;">foreach</span> (<span style="color: #0000ff;">var</span> o <span style="color: #0000ff;">in</span><span style="color: #000000;"> driveId.Get())
            {
                </span><span style="color: #0000ff;">var</span> mo =<span style="color: #000000;"> (ManagementObject)o;
                st </span>= mo[<span style="color: #800000;">"</span><span style="color: #800000;">Name</span><span style="color: #800000;">"</span><span style="color: #000000;">].ToString();
            }
            </span><span style="color: #0000ff;">return</span><span style="color: #000000;"> st;
        }

        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;summary&gt;</span>  
        <span style="color: #808080;">///</span><span style="color: #008000;"> CPU制造厂商  
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;/summary&gt;</span>  
        <span style="color: #808080;">///</span> <span style="color: #808080;">&lt;returns&gt;&lt;/returns&gt;</span>  
        <span style="color: #0000ff;">public</span> <span style="color: #0000ff;">static</span> <span style="color: #0000ff;">string</span><span style="color: #000000;"> GetCpuManufacturer()
        {
            </span><span style="color: #0000ff;">var</span> st = <span style="color: #0000ff;">string</span><span style="color: #000000;">.Empty;
            </span><span style="color: #0000ff;">var</span> mos = <span style="color: #0000ff;">new</span> ManagementObjectSearcher(<span style="color: #800000;">"</span><span style="color: #800000;">Select * from Win32_Processor</span><span style="color: #800000;">"</span><span style="color: #000000;">);
            </span><span style="color: #0000ff;">foreach</span> (<span style="color: #0000ff;">var</span> o <span style="color: #0000ff;">in</span><span style="color: #000000;"> mos.Get())
            {
                </span><span style="color: #0000ff;">var</span> mo =<span style="color: #000000;"> (ManagementObject)o;
                st </span>= mo[<span style="color: #800000;">"</span><span style="color: #800000;">Manufacturer</span><span style="color: #800000;">"</span><span style="color: #000000;">].ToString();
            }
            </span><span style="color: #0000ff;">return</span><span style="color: #000000;"> st;
        }
        </span><span style="color: #0000ff;">#endregion</span>

        <span style="color: #0000ff;">#region</span> 主板
        <span style="color: #808080;">///</span> <span style="color: #808080;">&lt;summary&gt;</span>  
        <span style="color: #808080;">///</span><span style="color: #008000;"> 主板制造厂商  
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;/summary&gt;</span>  
        <span style="color: #808080;">///</span> <span style="color: #808080;">&lt;returns&gt;&lt;/returns&gt;</span>  
        <span style="color: #0000ff;">public</span> <span style="color: #0000ff;">static</span> <span style="color: #0000ff;">string</span><span style="color: #000000;"> GetBoardManufacturer()
        {
            </span><span style="color: #0000ff;">var</span> query = <span style="color: #0000ff;">new</span> SelectQuery(<span style="color: #800000;">"</span><span style="color: #800000;">Select * from Win32_BaseBoard</span><span style="color: #800000;">"</span><span style="color: #000000;">);
            </span><span style="color: #0000ff;">var</span> mos = <span style="color: #0000ff;">new</span><span style="color: #000000;"> ManagementObjectSearcher(query);
            </span><span style="color: #0000ff;">var</span> data =<span style="color: #000000;"> mos.Get().GetEnumerator();
            data.MoveNext();
            </span><span style="color: #0000ff;">var</span> board =<span style="color: #000000;"> data.Current;
            </span><span style="color: #0000ff;">return</span> board.GetPropertyValue(<span style="color: #800000;">"</span><span style="color: #800000;">Manufacturer</span><span style="color: #800000;">"</span><span style="color: #000000;">).ToString();
        }

        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;summary&gt;</span>  
        <span style="color: #808080;">///</span><span style="color: #008000;"> 主板编号  
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;/summary&gt;</span>  
        <span style="color: #808080;">///</span> <span style="color: #808080;">&lt;returns&gt;&lt;/returns&gt;</span>  
        <span style="color: #0000ff;">public</span> <span style="color: #0000ff;">static</span> <span style="color: #0000ff;">string</span><span style="color: #000000;"> GetBoardId()
        {
            </span><span style="color: #0000ff;">var</span> st = <span style="color: #0000ff;">string</span><span style="color: #000000;">.Empty;
            </span><span style="color: #0000ff;">var</span> mos = <span style="color: #0000ff;">new</span> ManagementObjectSearcher(<span style="color: #800000;">"</span><span style="color: #800000;">Select * from Win32_BaseBoard</span><span style="color: #800000;">"</span><span style="color: #000000;">);
            </span><span style="color: #0000ff;">foreach</span> (<span style="color: #0000ff;">var</span> o <span style="color: #0000ff;">in</span><span style="color: #000000;"> mos.Get())
            {
                </span><span style="color: #0000ff;">var</span> mo =<span style="color: #000000;"> (ManagementObject)o;
                st </span>= mo[<span style="color: #800000;">"</span><span style="color: #800000;">SerialNumber</span><span style="color: #800000;">"</span><span style="color: #000000;">].ToString();
            }
            </span><span style="color: #0000ff;">return</span><span style="color: #000000;"> st;
        }

        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;summary&gt;</span>  
        <span style="color: #808080;">///</span><span style="color: #008000;"> 主板型号  
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;/summary&gt;</span>  
        <span style="color: #808080;">///</span> <span style="color: #808080;">&lt;returns&gt;&lt;/returns&gt;</span>  
        <span style="color: #0000ff;">public</span> <span style="color: #0000ff;">static</span> <span style="color: #0000ff;">string</span><span style="color: #000000;"> GetBoardType()
        {
            </span><span style="color: #0000ff;">var</span> st = <span style="color: #0000ff;">string</span><span style="color: #000000;">.Empty;
            </span><span style="color: #0000ff;">var</span> mos = <span style="color: #0000ff;">new</span> ManagementObjectSearcher(<span style="color: #800000;">"</span><span style="color: #800000;">Select * from Win32_BaseBoard</span><span style="color: #800000;">"</span><span style="color: #000000;">);
            </span><span style="color: #0000ff;">foreach</span> (<span style="color: #0000ff;">var</span> o <span style="color: #0000ff;">in</span><span style="color: #000000;"> mos.Get())
            {
                </span><span style="color: #0000ff;">var</span> mo =<span style="color: #000000;"> (ManagementObject)o;
                st </span>= mo[<span style="color: #800000;">"</span><span style="color: #800000;">Product</span><span style="color: #800000;">"</span><span style="color: #000000;">].ToString();
            }
            </span><span style="color: #0000ff;">return</span><span style="color: #000000;"> st;
        }

        </span><span style="color: #0000ff;">#endregion</span>


        <span style="color: #0000ff;">#region</span> 网卡，声卡，显卡
        <span style="color: #808080;">///</span> <span style="color: #808080;">&lt;summary&gt;</span>  
        <span style="color: #808080;">///</span><span style="color: #008000;"> 获取网卡硬件地址  
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;/summary&gt;</span>  
        <span style="color: #808080;">///</span> <span style="color: #808080;">&lt;returns&gt;&lt;/returns&gt;</span>   
        <span style="color: #0000ff;">public</span> <span style="color: #0000ff;">static</span> <span style="color: #0000ff;">string</span><span style="color: #000000;"> GetMacAddress()
        {
            </span><span style="color: #0000ff;">var</span> mac = <span style="color: #800000;">""</span><span style="color: #000000;">;
            </span><span style="color: #0000ff;">var</span> mc = <span style="color: #0000ff;">new</span> ManagementClass(<span style="color: #800000;">"</span><span style="color: #800000;">Win32_NetworkAdapterConfiguration</span><span style="color: #800000;">"</span><span style="color: #000000;">);
            </span><span style="color: #0000ff;">var</span> moc =<span style="color: #000000;"> mc.GetInstances();
            </span><span style="color: #0000ff;">foreach</span> (<span style="color: #0000ff;">var</span> o <span style="color: #0000ff;">in</span><span style="color: #000000;"> moc)
            {
                </span><span style="color: #0000ff;">var</span> mo =<span style="color: #000000;"> (ManagementObject)o;
                </span><span style="color: #0000ff;">if</span> (!(<span style="color: #0000ff;">bool</span>)mo[<span style="color: #800000;">"</span><span style="color: #800000;">IPEnabled</span><span style="color: #800000;">"</span>]) <span style="color: #0000ff;">continue</span><span style="color: #000000;">;
                mac </span>= mo[<span style="color: #800000;">"</span><span style="color: #800000;">MacAddress</span><span style="color: #800000;">"</span><span style="color: #000000;">].ToString();
                </span><span style="color: #0000ff;">break</span><span style="color: #000000;">;
            }
            </span><span style="color: #0000ff;">return</span><span style="color: #000000;"> mac;
        }

        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;summary&gt;</span>  
        <span style="color: #808080;">///</span><span style="color: #008000;"> 显卡PNPDeviceID  
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;/summary&gt;</span>  
        <span style="color: #808080;">///</span> <span style="color: #808080;">&lt;returns&gt;&lt;/returns&gt;</span>  
        <span style="color: #0000ff;">public</span> <span style="color: #0000ff;">static</span> <span style="color: #0000ff;">string</span><span style="color: #000000;"> GetVideoPnpid()
        {
            </span><span style="color: #0000ff;">var</span> st = <span style="color: #800000;">""</span><span style="color: #000000;">;
            </span><span style="color: #0000ff;">var</span> mos = <span style="color: #0000ff;">new</span> ManagementObjectSearcher(<span style="color: #800000;">"</span><span style="color: #800000;">Select * from Win32_VideoController</span><span style="color: #800000;">"</span><span style="color: #000000;">);
            </span><span style="color: #0000ff;">foreach</span> (<span style="color: #0000ff;">var</span> o <span style="color: #0000ff;">in</span><span style="color: #000000;"> mos.Get())
            {
                </span><span style="color: #0000ff;">var</span> mo =<span style="color: #000000;"> (ManagementObject)o;
                st </span>= mo[<span style="color: #800000;">"</span><span style="color: #800000;">PNPDeviceID</span><span style="color: #800000;">"</span><span style="color: #000000;">].ToString();
            }
            </span><span style="color: #0000ff;">return</span><span style="color: #000000;"> st;
        }

        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;summary&gt;</span>  
        <span style="color: #808080;">///</span><span style="color: #008000;"> 声卡PNPDeviceID  
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;/summary&gt;</span>  
        <span style="color: #808080;">///</span> <span style="color: #808080;">&lt;returns&gt;&lt;/returns&gt;</span>  
        <span style="color: #0000ff;">public</span> <span style="color: #0000ff;">static</span> <span style="color: #0000ff;">string</span><span style="color: #000000;"> GetSoundPnpid()
        {
            </span><span style="color: #0000ff;">var</span> st = <span style="color: #0000ff;">string</span><span style="color: #000000;">.Empty;
            </span><span style="color: #0000ff;">var</span> mos = <span style="color: #0000ff;">new</span> ManagementObjectSearcher(<span style="color: #800000;">"</span><span style="color: #800000;">Select * from Win32_SoundDevice</span><span style="color: #800000;">"</span><span style="color: #000000;">);
            </span><span style="color: #0000ff;">foreach</span> (<span style="color: #0000ff;">var</span> o <span style="color: #0000ff;">in</span><span style="color: #000000;"> mos.Get())
            {
                </span><span style="color: #0000ff;">var</span> mo =<span style="color: #000000;"> (ManagementObject)o;
                st </span>= mo[<span style="color: #800000;">"</span><span style="color: #800000;">PNPDeviceID</span><span style="color: #800000;">"</span><span style="color: #000000;">].ToString();
            }
            </span><span style="color: #0000ff;">return</span><span style="color: #000000;"> st;
        } 
        </span><span style="color: #0000ff;">#endregion</span>



        <span style="color: #808080;">///</span> <span style="color: #808080;">&lt;summary&gt;</span>  
        <span style="color: #808080;">///</span><span style="color: #008000;"> 获取硬盘序列号  
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;/summary&gt;</span>  
        <span style="color: #808080;">///</span> <span style="color: #808080;">&lt;returns&gt;&lt;/returns&gt;</span>  
        <span style="color: #0000ff;">public</span> <span style="color: #0000ff;">static</span> <span style="color: #0000ff;">string</span><span style="color: #000000;"> GetDiskSerialNumber()
        {
            </span><span style="color: #008000;">//</span><span style="color: #008000;">这种模式在插入一个U盘后可能会有不同的结果，如插入我的手机时  </span>
            <span style="color: #0000ff;">var</span> hDid = <span style="color: #0000ff;">string</span><span style="color: #000000;">.Empty;
            </span><span style="color: #0000ff;">var</span> mc = <span style="color: #0000ff;">new</span> ManagementClass(<span style="color: #800000;">"</span><span style="color: #800000;">Win32_DiskDrive</span><span style="color: #800000;">"</span><span style="color: #000000;">);
            </span><span style="color: #0000ff;">var</span> moc =<span style="color: #000000;"> mc.GetInstances();
            </span><span style="color: #0000ff;">foreach</span> (<span style="color: #0000ff;">var</span> o <span style="color: #0000ff;">in</span><span style="color: #000000;"> moc)
            {
                </span><span style="color: #0000ff;">var</span> mo =<span style="color: #000000;"> (ManagementObject)o;
                hDid </span>= (<span style="color: #0000ff;">string</span>)mo.Properties[<span style="color: #800000;">"</span><span style="color: #800000;">Model</span><span style="color: #800000;">"</span><span style="color: #000000;">].Value;
                </span><span style="color: #008000;">//</span><span style="color: #008000;">这名话解决有多个物理盘时产生的问题，只取第一个物理硬盘  </span>
                <span style="color: #0000ff;">break</span><span style="color: #000000;">;
            }
            </span><span style="color: #0000ff;">return</span><span style="color: #000000;"> hDid;
        }

        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;summary&gt;</span>
        <span style="color: #808080;">///</span><span style="color: #008000;"> 获取硬盘的大小
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;/summary&gt;</span>
        <span style="color: #808080;">///</span> <span style="color: #808080;">&lt;returns&gt;&lt;/returns&gt;</span>
        <span style="color: #0000ff;">public</span> <span style="color: #0000ff;">static</span> <span style="color: #0000ff;">string</span><span style="color: #000000;"> GetSizeOfDisk()
        {
            ManagementClass mc </span>= <span style="color: #0000ff;">new</span> ManagementClass(<span style="color: #800000;">"</span><span style="color: #800000;">Win32_DiskDrive</span><span style="color: #800000;">"</span><span style="color: #000000;">);
            ManagementObjectCollection moj </span>=<span style="color: #000000;"> mc.GetInstances();
            </span><span style="color: #0000ff;">foreach</span> (ManagementObject m <span style="color: #0000ff;">in</span><span style="color: #000000;"> moj)
            {
                </span><span style="color: #0000ff;">return</span> m.Properties[<span style="color: #800000;">"</span><span style="color: #800000;">Size</span><span style="color: #800000;">"</span><span style="color: #000000;">].Value.ToString();
            }
            </span><span style="color: #0000ff;">return</span> <span style="color: #800000;">"</span><span style="color: #800000;">-1</span><span style="color: #800000;">"</span><span style="color: #000000;">;
        }


        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;summary&gt;</span>  
        <span style="color: #808080;">///</span><span style="color: #008000;"> 物理内存  
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;/summary&gt;</span>  
        <span style="color: #808080;">///</span> <span style="color: #808080;">&lt;returns&gt;&lt;/returns&gt;</span>  
        <span style="color: #0000ff;">public</span> <span style="color: #0000ff;">static</span> <span style="color: #0000ff;">string</span><span style="color: #000000;"> GetPhysicalMemory()
        {
            </span><span style="color: #0000ff;">var</span> st = <span style="color: #0000ff;">string</span><span style="color: #000000;">.Empty;
            </span><span style="color: #0000ff;">var</span> mc = <span style="color: #0000ff;">new</span> ManagementClass(<span style="color: #800000;">"</span><span style="color: #800000;">Win32_ComputerSystem</span><span style="color: #800000;">"</span><span style="color: #000000;">);
            </span><span style="color: #0000ff;">var</span> moc =<span style="color: #000000;"> mc.GetInstances();
            </span><span style="color: #0000ff;">foreach</span> (<span style="color: #0000ff;">var</span> o <span style="color: #0000ff;">in</span><span style="color: #000000;"> moc)
            {
                </span><span style="color: #0000ff;">var</span> mo =<span style="color: #000000;"> (ManagementObject)o;
                st </span>= mo[<span style="color: #800000;">"</span><span style="color: #800000;">TotalPhysicalMemory</span><span style="color: #800000;">"</span><span style="color: #000000;">].ToString();
            }
            </span><span style="color: #0000ff;">return</span><span style="color: #000000;"> st;
        }




        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;summary&gt;</span>  
        <span style="color: #808080;">///</span><span style="color: #008000;"> 获取IP地址  
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;/summary&gt;</span>  
        <span style="color: #808080;">///</span> <span style="color: #808080;">&lt;returns&gt;&lt;/returns&gt;</span>  
        <span style="color: #0000ff;">public</span> <span style="color: #0000ff;">static</span> <span style="color: #0000ff;">string</span><span style="color: #000000;"> GetIpAddress()
        {
            </span><span style="color: #0000ff;">var</span> st = <span style="color: #0000ff;">string</span><span style="color: #000000;">.Empty;
            </span><span style="color: #0000ff;">var</span> mc = <span style="color: #0000ff;">new</span> ManagementClass(<span style="color: #800000;">"</span><span style="color: #800000;">Win32_NetworkAdapterConfiguration</span><span style="color: #800000;">"</span><span style="color: #000000;">);
            </span><span style="color: #0000ff;">var</span> moc =<span style="color: #000000;"> mc.GetInstances();
            </span><span style="color: #0000ff;">foreach</span> (<span style="color: #0000ff;">var</span> o <span style="color: #0000ff;">in</span><span style="color: #000000;"> moc)
            {
                </span><span style="color: #0000ff;">var</span> mo =<span style="color: #000000;"> (ManagementObject)o;
                </span><span style="color: #0000ff;">if</span> (!(<span style="color: #0000ff;">bool</span>)mo[<span style="color: #800000;">"</span><span style="color: #800000;">IPEnabled</span><span style="color: #800000;">"</span>]) <span style="color: #0000ff;">continue</span><span style="color: #000000;">;
                </span><span style="color: #0000ff;">var</span> ar = (Array)(mo.Properties[<span style="color: #800000;">"</span><span style="color: #800000;">IpAddress</span><span style="color: #800000;">"</span><span style="color: #000000;">].Value);
                st </span>= ar.GetValue(<span style="color: #800080;">0</span><span style="color: #000000;">).ToString();
                </span><span style="color: #0000ff;">break</span><span style="color: #000000;">;
            }
            </span><span style="color: #0000ff;">return</span><span style="color: #000000;"> st;
        }

        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;summary&gt;</span>  
        <span style="color: #808080;">///</span><span style="color: #008000;"> 操作系统的登录用户名  
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;/summary&gt;</span>  
        <span style="color: #808080;">///</span> <span style="color: #808080;">&lt;returns&gt;&lt;/returns&gt;</span>   
        <span style="color: #0000ff;">public</span> <span style="color: #0000ff;">static</span> <span style="color: #0000ff;">string</span><span style="color: #000000;"> GetUserName()
        {
            </span><span style="color: #0000ff;">return</span><span style="color: #000000;"> Environment.UserName;
        }

        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;summary&gt;</span>  
        <span style="color: #808080;">///</span><span style="color: #008000;"> 获取计算机名  
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;/summary&gt;</span>  
        <span style="color: #808080;">///</span> <span style="color: #808080;">&lt;returns&gt;&lt;/returns&gt;</span>  
        <span style="color: #0000ff;">public</span> <span style="color: #0000ff;">static</span> <span style="color: #0000ff;">string</span><span style="color: #000000;"> GetComputerName()
        {
            </span><span style="color: #0000ff;">return</span><span style="color: #000000;"> Environment.MachineName;
        }

        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;summary&gt;</span>  
        <span style="color: #808080;">///</span><span style="color: #008000;"> 操作系统类型  
        </span><span style="color: #808080;">///</span> <span style="color: #808080;">&lt;/summary&gt;</span>  
        <span style="color: #808080;">///</span> <span style="color: #808080;">&lt;returns&gt;&lt;/returns&gt;</span>   
        <span style="color: #0000ff;">public</span> <span style="color: #0000ff;">static</span> <span style="color: #0000ff;">string</span><span style="color: #000000;"> GetSystemType()
        {
            </span><span style="color: #0000ff;">var</span> st = <span style="color: #0000ff;">string</span><span style="color: #000000;">.Empty;
            </span><span style="color: #0000ff;">var</span> mc = <span style="color: #0000ff;">new</span> ManagementClass(<span style="color: #800000;">"</span><span style="color: #800000;">Win32_ComputerSystem</span><span style="color: #800000;">"</span><span style="color: #000000;">);
            </span><span style="color: #0000ff;">var</span> moc =<span style="color: #000000;"> mc.GetInstances();
            </span><span style="color: #0000ff;">foreach</span> (<span style="color: #0000ff;">var</span> o <span style="color: #0000ff;">in</span><span style="color: #000000;"> moc)
            {
                </span><span style="color: #0000ff;">var</span> mo =<span style="color: #000000;"> (ManagementObject)o;
                st </span>= mo[<span style="color: #800000;">"</span><span style="color: #800000;">SystemType</span><span style="color: #800000;">"</span><span style="color: #000000;">].ToString();
            }
            </span><span style="color: #0000ff;">return</span><span style="color: #000000;"> st;
        }
    }
}</span></pre>
</div>
<p>&nbsp;</p>