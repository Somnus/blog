
什么是Session？简单讲，Session是一种服务端用于保存每个客户端用户的状态信息的机制。客户端第一次访问时，服务端从分配一个空间专门存储该客户端的信息，后续访问时便可以直接获取或者更新状态信息。具体关于Session定义请查看参考。

###### .NET下Session使用很简单，对写入Session值无类型限制，设置Session有效时间也很简单，直接设置Session的timeout属性即可，类型为int,单位为分钟。
```
		Session["time"] = DateTime.Now.ToString("yyyymmddHHMMss");
		Session.Timeout = 60;'
```
###### 但若要设置多个有效时间不同的Session，就不那么友好了，后设置的有效时间会覆盖之前值。
```
		//写入用户登录状态到Session。
		Session["state"] = "On";
		//设置用户登录状态有效时间为1天，除非登录清空Session,否则整天登录状态有效。
        	Session.Timeout = 60 * 24;
        	//设置类似验证码类似的session,假设有效时间为1分钟。
		Session["time"] = DateTime.Now.ToString("yyyymmddHHMMss");
       		Session.Timeout = 1;
```	
###### 原因在于C#中只提供了session有效时间统一设置方法，无法对单个session进行有效时间进行设置。

###### 因此要在服务端对多个session分别设置有效时间就只能另辟蹊径，可以采用的方法大致分为两类：
- 基于session机制进行扩展，通过扩展方法实现，即方案1和方案3。目前并没有从博客园或者相关文献中找到方案，唯一找到的参考资料方案3中有提及。
- 考虑使用redis等替代session功能，redis允许对特定键值对设置有效时间，方案2。该类型的方案较多，博客园各位大大都有文献介绍，所以本文重点不在这里。

###### 方案1：构建结构为：  
```
		Tuple.Create<string, object, int>(key, value, time)
```	
###### 的tuple元组，将tuple写入Session中。写入时根据传入有效时间计算出过期时间，将过期时间存入，获取Session时判断是否已经超过过期截止时间，超过返回空，并且清空Session。
```
		private void SetSingleSession(string key, object value, int? timeout)
		{
			int time = timeout ?? Session.Timeout;
			DateTime endTime = DateTime.Now + new TimeSpan(0,time,0);
			var tuple = Tuple.Create(key, value, endTime);
			Session[key] = tuple;
		}
		
        	private object GetSingleSession(string key)
        	{
            		var tuple = Session[key] as Tuple<string, object, DateTime>;
            		var diff = DateTime.Compare(tuple.Item3, DateTime.Now);
            		object result = null;
            		if (diff > 0) result = tuple.Item2;
            		else Session.Remove(key);
            		return result;
       		}
```	
###### 方案2：使用Redis。
- 安装redis服务。windows版：[下载](https://github.com/MicrosoftArchive/redis/releases)，下载安装包并点击安装，直到安装完成。
- 设置登录密码，打开cmd窗口，输入“cd C:\Program Files\redis”进入redis安装目录，输入“redis-cli.exe”运行该exe程序，继续输入 “config pass requirepass password”（password改为要设置的登录密码），完成密码设置。
- Nuget中安装“StackExchange.Redis”.
- redis使用，基础代码如下。
```
		ConnectionMultiplexer redis = ConnectionMultiplexer.Connect("localhost:6379,password=fxy123");
        	IDatabase db = redis.GetDatabase();
        	//写入记录，包括有效时间：10秒过期
        	db.StringSet ( "key_test" , "shaocan",TimeSpan.FromSeconds(10));
```	
###### 方案3：通过扩展Session类，设置一个异步延迟执行机制，定时清除session。(原始文献参考[ASP.NET Session: Caching Expiring Values](https://www.jitbit.com/alexblog/196-aspnet-session-caching-expiring-values/))
```
    		public static void AddWithTimeout(this HttpSessionStateBase session,string name,object value,int? minute)
    		{
			TimeSpan timeSpan=TimeSpan.FromMinutes(minute??session.TimeOut);
			lock (session)
        		{
            			session[name] = value;
        		}    
        		Task.Delay(expireAfter).ContinueWith((task) => 
			{
            			lock (session)
            			{
                			session.Remove(name);
            			}
    			});
		}
```		
######  如果不是特别庞大的项目，推荐使用方案三，简单扩展方法即可实现，只需设置时使用Session扩展方法即可；对于比较大的项目，推荐使用方案2进行管理，毕竟Session机制由于服务器重启等原因会丢失，造成用户体验不佳。不推荐使用方案1，如果使用方案1设置和获取session都必须采用该扩展类进行，否则会存在过期但并未失效的情况。
