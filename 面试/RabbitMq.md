# RabbitMq

## 1. 安装

1. yum install erlang

2. wget http://erlang.org/download/otp_src_19.0.tar.gz

   1. 安装后，执行erl测试
      1. 通过halt().   退出命令行

3. yum install gcc glibc-devel make ncurses-devel openssl-devel xmlto

4. wget http://www.rabbitmq.com/releases/rabbitmq-server/v3.6.1/rabbitmq-server-generic-unix-3.6.1.tar.xz

5. rabbit服务

   1. rabbitmq-server  -detached   启动
   2. rabbitmqctl   status   查看状态
   3. rabbitmqctl    stop     停止服务

6. 安装web插件

   1. mkdir   /etc/rabbitmq
   2. rabbitmq-plugins enable rabbitmq_management 
   3. iptables -I INPUT -t nat -m tcp -p tcp --dport 1:65535 -j ACCEPT
   4. rabbitmqctl add_user gq gq
   5. rabbitmqctl set_permissions -p "/" gq ".*" ".*" ".*"
   6. rabbitmqctl set_user_tags gq administrator

7. rabbitmqctl常用命令

   1. 　　add_user        <UserName> <Password>

      　　delete_user     <UserName>

      　　change_password <UserName> <NewPassword>

      　　list_users

      　　add_vhost    <VHostPath>

      　　delete_vhost <VHostPath>

      　　list_vhostsset_permissions   [-p <VHostPath>] <UserName> <Regexp> <Regexp> <Regexp>

      　　clear_permissions [-p <VHostPath>] <UserName>

      　　list_permissions  [-p <VHostPath>]

      　　list_user_permissions <UserName>

      　　list_queues    [-p <VHostPath>] [<QueueInfoItem> ...]

      　　list_exchanges [-p <VHostPath>] [<ExchangeInfoItem> ...]

      　　list_bindings  [-p <VHostPath>]

      　　list_connections [<ConnectionInfoItem> ...]

8. 集群安装

   1. hostnamectl  set-hostname node1    // 修改主机名
   2. 同步.erlang.cookie文件，rabbitmqctl使用erlang运行，多机通信会调用erlang系统，需要.erlang.cookie密码
   3. rabbitmqctl stop
   4. rabbitmq-server -detached
   5. rabbitmqctl stop_app
   6. rabbitmqctl reset 
   7. rabbitmqctl join_cluster rabbit@node1
   8. rabbitmqctl start_app
   9. rabbitmqctl cluster_status

9. 集群分类

   1. 普通集群
      1. 集群之间只共享了元数据（exchange/queue/vhost/binding等元数据），queue保留在节点中，访问时，如果不在本节点，本节点作为一个中转节点，量大的时候，会有性能问题
   2. 镜像集群
      1. 各节点之间同步所有数据
      2. 集群内部大量通讯占用带宽
      3. 镜像集群通过vhost策略（policy）模块实现
         1. 如：rabbitmqctl set_policy -p vhostName ha-allqueue"^queue" '{"ha-mode":"all"}'
         2. all表示复制给所有节点
      4. 

10. 节点类型

    1. 磁盘节点
       1. 数据会保存在磁盘中，可以用来作为安全备份，最好别向用户提供
       2. 将元数据存储在磁盘中，单节点系统只允许磁盘类型的节点，防止重启 RabbitMQ 的时候，丢失系统的配置信息。
    2. 内存节点
       1. 元数据和信息保存在内存中，可以提供给用户（ha时，只配置到这里）

11. vhost

    1. 本质上是一个mini的RabbitMq服务器（类似于虚拟机），有独立的exchange, queue,binding等，在各个实例间提供逻辑上分离
    2. 可以实现不同应用之间安全的信息运行（保证了隔离性）

