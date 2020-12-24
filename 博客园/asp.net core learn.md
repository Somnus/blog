# .NET Core WebApi

### RESTful规范
- [RESTful API 最佳实践](http://www.ruanyifeng.com/blog/2018/10/restful-api-best-practices.html)
- [理解RESTful架构](http://www.ruanyifeng.com/blog/2011/09/restful.html)


### 接口版本控制
- [Support multiple versions of ASP.NET Core Web API](https://www.talkingdotnet.com/support-multiple-versions-of-asp-net-core-web-api/)
- [ASP.NET Core API 版本控制](https://www.cnblogs.com/tdfblog/p/asp-net-core-api-versioning.html)
- 配置使用流程
    - 使用方法
        - Startup类ConfigureServices方法中加入services.AddApiVersioning（）
        - Startup类Configure方法中加入app.UseApiVersioning()
        - 控制器或接口上面应用添加ApiVersion("1.0")特性。
    - 访问形式
        - 通过查询字符串方式实现访问，具体格式：`api/values?api-version=1.0`
        - 通过重写接口路由方式实现访问，路由修改为：[Route("api/v{version:apiVersion}/[controller]")]；具体格式：`api/v1/values`
        - 通过Header指定字段实现访问。

### Swagger
- [.NetCore WebApi —— Swagger版本控制](https://www.cnblogs.com/jixiaosa/p/10817143.html)
- [.NetCore WebApi——Swagger简单配置](https://www.cnblogs.com/jixiaosa/p/10759832.html)

### 跨域策略


### 依赖注入
- [ASP.NET Core依赖注入使用自带的IOC容器](https://www.itsvse.com/thread-7562-1-1.html)
- [ASP.NET Core使用Autofac实现IOC注入](https://www.itsvse.com/thread-7563-1-1.html)
- [ASP.NET Core使用Autofac实现AOP拦截](https://www.itsvse.com/thread-7566-1-1.html)


# 数据库
- [MySQL InnoDB锁机制全面解析分享](https://segmentfault.com/a/1190000014133576)

##### 事务特性
- 持久性：事务一旦被提交，那么数据一定会被写入到数据库中并持久储存起来。
- 原子性：每个事务可能由多个操作构成，但把事务本身看作操作的最小单位，一个事务中的操作不可能同时存在成功和失败两种状态，要么全部成功，否则恢复到事务执行之前状态。
    - Active 事务的初始状态，表示正在执行。
    - Partially Commited 部分执行，或者说在最后一条语句执行后。
    - Failed 发现操作异常，事务无法继续执行后。
    - Commited 成功执行整个事务。
    - Aborted 事务被回滚，数据库恢复到执行前状态后。
- 一致性：事务执行前后数据都满足数据完整性约束，完整性约束可能包括主键约束、外键约束、用户自定义约束。事务内部操作执行过程中可能不满足完整性约束，但一致性强调事务执行前后是否满足，忽略中间过程。
- 隔离性：一个事务所做的修改对其他事务不可见。

##### 事务隔离级别
- 读未提交`READ UNCOMMITTED` 事务中数据更改未提交也对其他事务可见。
    - 脏读
    - 不可重复读
    - 幻读
    - 更新丢失
- 读已提交`READ COMMITTED` 事务中数据更改已提交对其他事务可见。
    - 不可重复读
    - 幻读
    - 更新丢失
- 可重复读`REPEATABLE READ` 事务中数据更改已提交对正在执行的事务不可见。
    - 幻读
    - 更新丢失
- 串行化`SERIALIZABLE` 强制所有事务串行执行。
读已提交与可重复读区别：正在执行的事务是否可见其他事务提交的数据更改，读已提交可见，可重复读不可见。
SqlServer默认事务隔离级别为`读已提交`;MySql默认事务隔离级别为`可重复读`。

##### 隔离特性导致问题
- 脏读：当前事务中读取到其他事务已修改未提交的数据，违反隔离性。
- 不可重复读：当前事务中读取到其他事务已修改已提交的数据，违反一致性。
- 幻读：因数据插入导致一个事务中读取查询获取结果不一致现象。
    - MVCC
    - Next-Key Lock
- 更新丢失：并发事务对同一条记录进行更新，出现后一次修改覆盖前一次修改情况。
    - MVCC
    - Record Lock

### 锁机制
###### 两段锁协议(2PL)
- 在对任何数据进行读、写操作之前，首先要申请并获得对该数据的封锁。
- 在释放一个封锁之后，事务不再申请和获得其它任何封锁。

##### 悲观锁机制
`先取锁再访问`
- 共享锁（Share Locks，S锁）：一个线程给数据加上共享锁后，其他线程只能读取数据，不能修改。
- 排它锁（eXclusive Locks，X锁）：一个线程给数据加上排它锁后，其他线程不能读取也不能修改。
- 意向锁 （Intent Locks）：在指定资源上获取锁时，先在上一层次结构上获取意向锁。主要作用是：当需要获取表锁或者页锁时，只需要判断是否存在表意向锁或者页意向锁就可以快速。
兼容性规则为：共享锁和共享锁兼容，排它锁和其他的锁都排斥。

##### 乐观锁机制
- 假设多并发的事务不会互相影响，各事务能够在不产生锁的情况下处理各自影响的那部分数据。
- 在提交数据更新之前，每个事务会先检查在该事务读取数据后，有没有其他事务又修改了该数据。如果其他事务有更新的话，正在提交的事务会进行回滚。



### 索引
[聚簇索引与非聚簇索引（也叫二级索引）](https://www.jianshu.com/p/fa8192853184)
##### 索引分类
- 存储结构区分
    - 聚集索引
    - 非聚集索引
- 数据唯一性区分
    - 唯一索引
    - 主键索引


### 编码格式及类型
###### char、varchar、text、nchar、nvarchar、ntext的区别？
- `char(n)`：定长；索引效率高；n 必须是一个介于 1 和 8,000 之间的数值,存储大小为 n 个字节。
- `varchar(n)`： 变长；索引效率低于char(n)，灵活；n 必须是一个介于 1 和 8,000 之间的数值，存储大小为输入数据的字节的实际长度。varchar类型的实际长度是它的值的实际长度+1。为什么“+1”呢？这一个字节用于保存实际使用了多大的长度。
- `text(n)`：变长；非Unicode数据；不用指定长度。最大长度为2^31-1(2,147,483,647)个字符。
- `nchar(n)`：定长；处理unicode数据类型；n 的值必须介于 1 与 4,000 之间。存储大小为 n 字节的两倍。
- `nvarchar(n)`:变长；处理unicode数据类型；n 的值必须介于 1 与 4,000 之间。字节的存储大小是所输入字符个数的两倍。所输入的数据字符长度可以为零。
- `ntext(n)`：变长；处理unicode数据类型；不用指定长度。
一般来说，如果含有中文字符，用nchar/nvarchar，如果纯英文和数字，用char/varchar。