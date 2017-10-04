环境：ubuntu15.10

一台master 两台slave。先在master上配置.  再通过虚拟机克隆两台slave

master-hadoop     192.168.72.131

slave1-hadoop       192.168.72.129

slave2-hadoop     192.168.72.130



把/etc/hosts改了，没用的都删了，只留四行，一行localhost，三行hadoop相关的

127.0.0.1            localhost

192.168.72.131  master-hadoop

192.168.72.129  slave1-hadoop

192.168.72.130 slave2-hadoop

首先设置ssh免登录

[walle@master-hadoop ~]$ ssh-keygen -t rsa

[walle@master-hadoop ~]$ cd /home/walle/.ssh/

[walle@master-hadoop .ssh]$ cat id_rsa.pub >> authorized_keys

[walle@master-hadoop .ssh]$ chmod 600 authorized_keys

[walle@master-hadoop .ssh]$ chmod 700 ../.ssh/

然后先把java安上  我之前的文章有

然后去apache官网下个hadoop。解压后重命名为hadoop，然后放到/usr/local下。

接下来要赋予hadoop文件夹权限。这点非常重要

cd /usr/local/
sudo chown -R walle ./hadoop                   # 修改文件权限
接下来修改环境变量/etc/profile

export HADOOP_HOME=/usr/local/hadoop

export PATH=$PATH:$HADOOP_HOME/bin

export PATH=$PATH:$HADOOP_HOME/sbin

export HADOOP_MAPRED_HOME=$HADOOP_HOME

export HADOOP_COMMON_HOME=$HADOOP_HOME

export HADOOP_HDFS_HOME=$HADOOP_HOME

export YARN_HOME=$HADOOP_HOME

source /etc/profile 立即生效

接下来就要修改配置文件了~

修改/usr/local/hadoop/etc/hadoop/hadoop-env.sh

# The java implementation to use.
export JAVA_HOME=/usr/local/java/jdk1.8.0_77

这部分要改成绝对路径。貌似是因为hadoop不完善的原因，原来的写法并不能生效。

修改slaves 加入

slave1-hadoop
slave2-hadoop

修改core-site.xml,在configuration标签中加入

<property>
<name>fs.defaultFS</name>
<value>hdfs://master-hadoop:9000</value>
</property>

<property>
<name>io.file.buffer.size</name>
<value>131072</value>
</property>

<property>
<name>hadoop.tmp.dir</name>
<value>file:/usr/local/hadoop/tmp</value>
</property>



修改hdfs-site.xml

<property>
<name>dfs.namenode.name.dir</name>
<value>file:/usr/local/hadoop/dfs/name</value>
</property>

<property>
<name>dfs.namenode.data.dir</name>
<value>file:/usr/local/hadoop/dfs/data</value>
</property>

<property>
<name>dfs.replication</name>
<value>2</value>
</property>

关于Hadoop配置项的一点说明：
虽然只需要配置 fs.defaultFS 和 dfs.replication 就可以运行（官方教程如此），不过若没有配置 hadoop.tmp.dir 参数，则默认使用的临时目录为 /tmp/hadoo-hadoop，而这个目录在重启时有可能被系统清理掉，导致必须重新执行 format 才行。所以我们进行了设置，同时也指定 dfs.namenode.name.dir 和 dfs.datanode.data.dir，否则在接下来的步骤中可能会出错。
修改yarn-site.xml

<property>
<name>yarn.resourcemanager.address</name>
<value>master-hadoop:8032</value>
</property>

<property>
<name>yarn.resourcemanager.scheduler.address</name>
<value>master-hadoop:8030</value>
</property>

<property>
<name>yarn.resourcemanager.resource-tracker.address</name>
<value>master-hadoop:8031</value>
</property>

<property>
<name>yarn.resourcemanager.admin.address</name>
<value>master-hadoop:8033</value>
</property>

<property>
<name>yarn.resourcemanager.webapp.address</name>
<value>master-hadoop:8088</value>
</property>

<property>
<name>yarn.nodemanager.aux-services</name>
<value>mapreduce_shuffle</value>
</property>

<property>
<name>yarn.nodemanager.aux-services.mapreduce.shuffle.class</name>
<value>org.apache.hadoop.mapred.ShuffleHandler</value>
</property>



配置mapred-site.xml

从模板文件复制一个xml，执行命令：

mv mapred-site.xml.template mapred-site.xml 然后修改之

<property>
<name>mapreduce.framework.name</name>
<value>yarn</value>
</property>

<property>
<name>mapreduce.jobhistory.address</name>
<value>master-hadoop:10020</value>
</property>

<property>
<name>mapreduce.jobhistory.webapp.address</name>
<value>master-hadoop:19888</value>
</property>

修改完毕

克隆虚拟机，生成slave节点

1.修改机器名，编辑/etc/hostname，文件内容改为slave1|slave2后重启系统。
2.在master上ssh连接slave1和slave2，测试免密码登陆是否成功，执行
ssh  slave1-hadoop
3.在master上启动hadoop,执行
start-all.sh

再在master上登陆http://192.168.72.131:8088/cluster/nodes



即可看到如下画面QQ图片20160401145210
