# IPFS

> interplanetary fs 面向全球，点对点的分布式文件系统
>
> 不存文件名，以hash作为键，将字节流存储在底层数据库中

## 1 命令

### 1.1  基础命令

1. ipfs init    // 初始化一个ipfs项目，默认在家目录下.ipfs中
2. ipfs  add   *.txt     // 上传文件
3. ipfs cat  fshash    // 查看hash值为fshash的文件内容
4. ipfs add -r  *    // 上传目录
5. ipfs  ls   fshas    // 查看目录
6. ipfs    daemon     // 启动一个文件服务器，默认监听8080
7. ipfs   get    fshash     // 下载文件   -o    指定文件名， -a   归档tar包     -C    调用gzip算法
8. ipfs   refs    fshash     // 查看引用

### 1.2 文件交互

- ipfs files mkdir - Make directories.
- ipfs files cp - Copy files into mfs.
- ipfs files flush [] - Flush a given path's data to disk.
- ipfs files ls [] - List directories in the local mutable namespace.
- ipfs files mv - Move files.
- ipfs files read - Read a file in a given mfs.
- ipfs files rm ... - Remove a file.
- ipfs files stat - Display file status.
- ipfs files write - Write to a mutable file in a given filesystem.

### 1.3 后台/配置

* ipfs  daemon      // 启动一个本地化服务

* ipfs config Addresses.Gateway /ip4/0.0.0.0/tcp/8080    // 修改监听端口

  * > 访问方式： http://x.x.x.x:8080/ipfs/文件hash     

* ipfs  config   show    // 查看所有的配置信息

### 1.4 网站(ipns)

* > 允许将节点ID（一个hash地址） 与某个项目进行绑定，使用这个ID对项目进行访问，绑定的项目可以进行迭代替换，实现固定ID 访问

* ipfs  name  publish  文件hash      // 将项目目录发送到ipns中，就是与一个ID进行绑定 ，通过重新发布完成网站内容迭代

* ipfs   id    // 查看本地的节点ID

### 1.5 网络命令 

```
  NETWORK COMMANDS
    id            Show info about IPFS peers
    bootstrap     Add or remove bootstrap peers
    swarm         Manage connections to the p2p network
    dht           Query the DHT for values or peers
    ping          Measure the latency of a connection
    diag          Print diagnostics
```

### 1.6 工具命令

```
TOOL COMMANDS
    config        Manage configuration
    version       Show ipfs version information
    update        Download and apply go-ipfs updates
    commands      List all available commands
```

## 2. API

### 2.1 js API

> https://github.com/ipfs/js-ipfs#install

verify retreat cross opinion panic burger rocket build flock offer exclude warrior