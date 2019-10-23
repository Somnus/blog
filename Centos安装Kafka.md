


#### 配置java运行环境
1、安装java

    yum install -y java-1.8.0-openjdk.x86_64

2、配置环境变量

    vim /etc/profile

输入`i`切换到编辑状态，然后在文件最后追加

    export JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.201.b09-2.el7_6.x86_64/jre
    export JRE_HOME=${JAVA_HOME}/jre
    export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib
    export PATH=${JAVA_HOME}/bin:$PATH

输入`Esc`退出编辑模式，输入`wq`保存退出。
输入命令

    source /etc/profile

使设置生效，执行完可通过echo $PATH命令查看是否添加成功,成功一般会输出

    /usr/lib/jvm/java-1.8.0-openjdk-1.8.0.201.b09-2.el7_6.x86_64/jre/bin:/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.201.b09-2.el7_6.x86_64/jre/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/root/bin

#### 安装Kafka

    cd /opt 
    wget http://mirrors.tuna.tsinghua.edu.cn/apache/kafka/2.2.0/kafka_2.12-2.2.0.tgz
    tar -zvxf ./kafka_2.12-2.2.0.tgz
    cd kafka_2.12-2.2.0
    bin/zookeeper-server-start.sh -daemon config/zookeeper.properties
    bin/kafka-server-start.sh config/server.properties








