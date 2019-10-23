


- 系统版本：CentOS 7.3
- 运行环境：.NET Core
- 数据库：MySQL
- 进程守护：Supervisor


## .NET Core环境
### 安装CentOS中.NET Core依赖库
	yum install libunwind
	yum install libicu 

### 注册Microsoft密钥,产品存储库并安装所需的依赖项。
	sudo rpm -Uvh https://packages.microsoft.com/config/rhel/7/packages-microsoft-prod.rpm

### 更新可用于安装的产品
	sudo yum update

### 安装.NET SDK
	sudo yum install dotnet-sdk-2.2


## MySQL数据库
### 获取安装包并完成安装
	wget http://dev.mysql.com/get/mysql-community-release-el7-5.noarch.rpm
	rpm -ivh mysql-community-release-el7-5.noarch.rpm
	yum install mysql-community-server

### 启动MySQL
	service mysqld restart

### 登录数据库
	mysql -u root

### 设置密码（将password换为自己的密码）
	set password for 'root'@'localhost' =password('password');

### 指定用户远程访问授权
	grant all privileges on *.* to root@'%'identified by 'password';

### 大小写不敏感设置
- 编辑```/etc/my.cnf```文件,在```[mysqld]```节下 添加 ```lower_case_table_names=1``` 参数,重启MySQL才生效。


## 进程守护
### 安装Supervisor
	yum install python-setuptools
	easy_install supervisor
	mkdir /etc/supervisor
	echo_supervisord_conf > /etc/supervisor/supervisord.conf
### 配置Supervisor
- 打开```/etc/supervisor/supervisord.conf```文件,将

		;[include]
		;files = relative/directory/*.ini
更换为

		[include]
		files = conf.d/*.conf

- 进入目录```/etc/supervisor/```新建```conf.d```文件夹,```conf.d```文件夹下新建 ```netcore.conf```文件,内容如下：

		[program:netcore]	
		command=dotnet mhqtalks.dll ; （注意）运行程序的命令	
		directory= /home/netcore/mhqtalks/ ; （注意 注意）对应的你的项目的存放目录，这个地方好多初学者搞错！！	
		autorestart=true ; 程序意外退出是否自动重启
		stderr_logfile=/var/log/netcore.err.log ; 错误日志文件
		stdout_logfile=/var/log/netcore.out.log ; 输出日志文件
		environment=ASPNETCORE_ENVIRONMENT=Production ; 进程环境变量
		user=root ; 进程执行的用户身份
		stopsignal=INT
执行命令

		supervisord -c /etc/supervisor/supervisord.conf 
		supervisorctl reload

- 配置Supervisor开机启动：打开目录 /usr/lib/systemd/system/ 新建文件 supervisord.service，内容如下：

		# dservice for systemd (CentOS 7.0+)
		# by ET-CS (https://github.com/ET-CS)
		[Unit]
		Description=Supervisor daemon

		[Service]
		Type=forking
		ExecStart=/usr/bin/supervisord -c /etc/supervisor/supervisord.conf
		ExecStop=/usr/bin/supervisorctl shutdown
		ExecReload=/usr/bin/supervisorctl reload
		KillMode=process
		Restart=on-failure
		RestartSec=42s
		
		[Install]
		WantedBy=multi-user.target
执行命令

		systemctl enable supervisord  
		systemctl is-enabled supervisord #来验证是否为开机启动

- 启动Supervisor

		supervisorctl start netcore


##参考资料
- 微软官方教程：[http://https://dotnet.microsoft.com/learn/dotnet/hello-world-tutorial](http://https://dotnet.microsoft.com/learn/dotnet/hello-world-tutorial "微软官方教程")
- ASP.NET Core 发布 CentOS 7 配置守护进程：[https://www.cnblogs.com/mhq-martin/p/8639166.html](https://www.cnblogs.com/mhq-martin/p/8639166.html "ASP.NET Core 发布 centos7 配置守护进程")
- CentOS 7 MySQL数据库安装和配置：[https://www.cnblogs.com/starof/p/4680083.html](https://www.cnblogs.com/starof/p/4680083.html "centos7 mysql数据库安装和配置")
