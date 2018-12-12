# fabric

## 简介

> 2015年linux基金会发起了Hyperledger项目
>
> fabric是Hyperledger中的一个区块链项目
>
> 与其他区块链技术相比：
>
> 1. fabric用于组建私有链和联盟链，通过MSP(Membership Service Provider)登记所有的成员
> 2. 账本可以被存储位多种格式
> 3. channel机制，允许参与者为交易创建一个独立的账本（只有在同一个channel中的成员才拥有这份账本），保证数据的私密性
>
> 共享账本：
>
> 1. fabric包含一个账本子系统，包含
>    1. 世界状态（world state）
>       1. 描述了账本在特定时间点的状态，是账本的数据库
>       2. 存储数据库是可替换的
>       3. 默认配置中是一个key-value存储数据库
>    2. 交易记录
>       1. 记录了产生世界状态当前值的所有交易，是世界状态的更新历史
> 2. 每个参与者拥有一个账本副本
>
> 智能合约（chaincode，链码）
>
> 1. 区块链外部访问账本的媒介
> 2. 一般只会访问账本的数据库组件和世界状态，而不会查询交易记录
> 3. go接口文档
>    1. https://godoc.org/github.com/hyperledger/fabric/core/chaincode/shim
>
> 背书策略：
>
> 1. fabric中通过实例化合约时的-P 参数，指定了背书策略
>    1. AND （”org1.member“，”org2.member“）
>    2. OR ("org1.member","org2.member")
>    3. AND ("org3.member", OR ("org1.member","org2.member"))
> 2. 某个节点发起交易提案的时候，指定背书节点，各节点进行模拟交易后，返回给发起节点，提交给orderer节点进行打包交易
> 3. 所以背书节点不是节点主动承担背书功能，而是提案发起方进行指定
> 4. 参与方的多寡，取决于实例化合约时指定的背书策略
>
>
>
> 隐私
>
> 1. 相较于其他区块链项目，支持了更多的隐私保护网络
>
> 共识
>
> 1. 目前支持SOLO，kafka，和后续可能加入的SBFT(Simplified Byzantine Fault Tolerance)
>

## 1 核心模块

### 1.1 peer 

> 主节点模块，负责存储区块链数据，维护链码（智能合约）
>
> 角色：
>
> 1. 锚节点
>    1. 组织间通信
> 2. leader节点
>    1. 只有leader节点才能链接orderer节点
>
> 链代码部署后，只需要在任意peer节点初始化一次，不需要在每个节点分别初始化。

## 1.2 orderer

> 交易打包，排序模块
>
> 不存储数据

### 1.3 cryptogen

> 组织和证书生成模块

#### 1.3.1 生成证书

> cryptogen generate --config=CONFIG
>
> **crypto-config/peerOrganizations/org1.gq.com/msp**
>
> msp 是一种角色管理框架，在生成的节点文件中，msp中保存了用户行为所需的各种认证文件/证书

#### 1.3.2 MSP

> fabric中使用一组证数和密钥文件表征以通成员的身份
>
> 1. 保证记录在区块链中的数据具有不可逆、不可篡改
> 2. 每条交易都会加上发起者的标签，同时发起人进行密钥加密
> 3. 如果作为背书节点，需要对信息进行签名
>
> peer/orderer 都有自己的MSP，单个节点中的user也都拥有自己的MSP
>
> 启动节点的时候，使用peer/orderer自己的MSP
>
> 创建通道，提议交易的时候使用用户的MSP
>
>

### 1.4 configtxgen

> 区块和交易生成模块
>
> 三种用法：
>
> 1. 生成Orderer服务启动的初始区块（即系统通道的创世区块）
>
> 2. 生成新建应用通道的配置交易（即用于创建应用通道的配置交易文件）
>
> 3. 生成锚节点配置更新文件
>
>    1. 锚节点：负责组织之间通信的代理节点
>    2. 锚节点只能有一个，所有的peer节点都可以作为锚节点
>

#### 1.4.1 创世区块和通道文件的生成

> 需要一个固定的配置文件configtx.yaml

### 1.5 configtxlator

> 区块和交易解析模块（可以将交易转译为json格式）

### 1.6 fabric-ca-client

> fabricCa用于管理fabric中的MSP（账户信息）
>
> 通常，一个组织对应一个fabric-CA服务器

## 2. 问题收集

### 2.1 /Channel/Application at version 0, but got version 1

> 因为创建一个已存在的channel，
>
> 解决方案：TODO
>
> ​	1. 文件保存在映射到本地的磁盘中，删除对应的文件即可
>
> 临时解决：重定义一个channelID

### 2.2 Error: error getting chaincode code mycc: path to chaincode does not exist

> 1. 路径指向文件在的文件夹，而不是文件
> 2. 使用的是相对路径：github.com/chaincode/chaincode_example02/go/（$GOPATH/src/后面开始）

### 2.3 API error (404): network _byfn not found

> CORE_VM_DOCKER_HOSTCONFIG_NETWORKMODE=gq_byfn
>
> peer节点环境变量需要正确设置

cms/crawler/jiankong