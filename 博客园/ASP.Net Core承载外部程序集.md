### 故事背景
&emsp;&emsp;一般情况下`ASP.Net Core`项目配置可以直接在`appsetting.json`中添加，也可以在项目中添加新的配置文件。但如果想和其他项目一起实现配置文件通用呢？我们可以用绝对定位去访问配置文件，但可能会遇到访问权限之类的问题；我们也可以通过开发配置文件访问接口来实现，但太麻烦了，而且不可能加了一个配置我就去改一次访问代码。那么，官方有木有提供什么方案呢？

&emsp;&emsp;有的，微软官方提供了允许`ASP.Net Core`承载外部程序集功能，实现逻辑就是通过外部类实现`IHostingStartup`接口，在启动时从外部程序集向应用添加增强功能。针对我们前面提到的外部项目向`ASP.Net Core`中添加配置文件需求是如何实现的呢？无非`ASP.Net Core`在启动时执行`启动依赖程序集`中指定特性的类中的`Configure`方法，而在该方法下将需要共享的配置添加到`ASP.Net Core`运行时中。

### 基本流程
##### 外部类库程序集
- 创建类库项目`HostingStartupLibrary`。
- 打开`Nuget`管理界面，依次安装`Microsoft.AspNetCore.Hosting(2.2.7)`、`Microsoft.Extensions.Configuration(3.0.0)`包。
- 新增承载类`ServiceKeyInjection`，实现`IHostingStartup`接口的`Configure`方法，添加部分数据到内存中。

    ```
    using System.Collections.Generic;
    using Microsoft.AspNetCore.Hosting;
    using Microsoft.Extensions.Configuration;

    [assembly: HostingStartup(typeof(HostingStartupLibrary.ServiceKeyInjection))]
    namespace HostingStartupLibrary
    {
        public class ServiceKeyInjection : IHostingStartup
        {
            var dict = new Dictionary<string, string>()
            {
                {"DevAccount_FromLibrary", "DEV_1111111-1111"}
            };

            //配置方法一：主项目配置优先加载，再加载当前配置。
            builder.ConfigureAppConfiguration(config =>
            { 
                config.AddInMemoryCollection(dict);
            });

            //配置方法二：当前配置优先加载，再加载主项目配置。
            //var builderConfig = new ConfigurationBuilder().AddInMemoryCollection(dict).Build();
            //builder.UseConfiguration(builderConfig); 
        }
    }
    ```

##### ASP.Net Core主项目
- 添加对类库项目`HostingStartupLibrary`的引用，也可以直接引用类库项目编译后的dll文件。
- 配置`主机启动依赖程序集`,配置方法有两种：主机配置与环境变量配置。若同时设置了主机配置与环境变量配置，则实际采用主机配置控制。
    - 主机配置
        - 打开`Program.cs`，找到`CreateHostBuilder`方法。
        - 在`UseStartup<Startup>()`之前，`webBuilder`之后添加`UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "HostingStartupLibrary")`,`HostingStartupLibrary`即为外部程序集的名称。

    - 环境变量配置
        - 打开`launchSettings.json`文件;
        - 找到所有的`environmentVariables`节点，在该节点下面添加`ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`,值为`HostingStartupLibrary`,即外部程序集名称。
- 配置获取测试。
    - 通过构造函数注入，将`IConfiguration`注入到控制器中。
    - 通过`config["DevAccount_FromLibrary"]`形式获取配置数据，判断是否正确。


### 详细解读