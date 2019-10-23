主数据库：47.106.241.221，centos7，mysql 5.6.43

从数据库：本地（公网ip未知），window10，mysql5.6.43


### 数据库安装
##### 主数据库安装
获取安装包并完成安装

	wget http://dev.mysql.com/get/mysql-community-release-el7-5.noarch.rpm
	rpm -ivh mysql-community-release-el7-5.noarch.rpm
	yum install mysql-community-server
	
启动MySQL

	service mysqld restart
修改初始密码

	mysql -u root
	set password for 'root'@'localhost' =password('password');
	quit;
##### 从数据库安装
下载mysql（window）5.6.43安装包，安装直到完成。

### 同步配置

##### 主库配置
	vi /etc/my.cnf
在[mysqld]节点下添加如下信息：

	log-bin=mysql-bin  #数据复制日志
	server-id=101 #保证在该主从同步机制中唯一
	binlog-do-db=xly #同步的库

按Esc推出编辑，输入

	:wq
保存。
重启mysql，

	service mysqld restart
登录mysql

	mysql -uroot -ppassword
创建用于数据同步的账号

	create user 'repl'@'%' identified by 'password';
数据同步账号授权

	grant replication slave on *.* to 'repl'@'%';
刷新权限

	flush privileges;
查看主库状态，并记录File与Position值

	show master status;

##### 从库配置
打开my.ini文件，在[server-id]节点下添加

	server-id=102 #设置server-id，保证当前主从同步机制中唯一
	binlog-do-db=xly #同步的库
重启 mysql，登录mysql，执行以下sql命令：

	CHANGE MASTER TO
	    MASTER_HOST='182.92.172.80', /*主数据库ip地址*/
	    MASTER_USER='rep1', /*主数据库同步账号*/
	    MASTER_PASSWORD='slavepass',/*主数据库同步密码*/
	    MASTER_LOG_FILE='mysql-bin.000003', /*主从复制配置/主库配置中记录的File值*/
	    MASTER_LOG_POS=73;/*主从复制配置/主库配置中记录的Position值*/
启动从库

	start slave;
检查从库状态

	show slave status;
检查`Slave_IO_Running`和`Slave_SQL_Running`是否都为`YES`，若是表示表示主从同步设置成功。

### 测试验证
在主库插入一条数据，查看从库是否增加。

在主库更改一条数据，查看从库是否修改。

在主库删除一条数据，看出从库是否删除。

### 注意事项
##### 同步设置之前一定要保证主从库的版本，表结构，表数据完成一致。
##### 每次重启主服务器都会导致MASTER STATUS修改，此时需要在从库也做对应修改。
