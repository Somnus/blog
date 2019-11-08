### 故事背景

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
- 配置`主机启动依赖程序集`,配置方法有两种：主机配置与环境变量配置。若同时设置了主机配置与环境变量配置，则实际采用主机配置控制
    - 主机配置
        - 打开`Program.cs`，找到`CreateHostBuilder`方法。
        - 在`UseStartup<Startup>()`之前，`webBuilder`之后添加`UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "HostingStartupLibrary")`,`HostingStartupLibrary`即为外部程序集的名称。

    - 环境变量配置
        - 打开`launchSettings.json`文件;
        - 找到所有的`environmentVariables`节点，在该节点下面添加`ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`,值为`HostingStartupLibrary`,即外部程序集名称。


### 详细解读
