# go-kafka

## 1. 准备

> 1. 安装依赖库sarama
>    go get github.com/Shopify/sarama
>    该库要求kafka版本在0.8及以上，支持kafka定义的high-level API和low-level API，但不支持常用的consumer自动rebalance和offset追踪，所以一般得结合cluster版本使用。
>
> 2. sarama-cluster依赖库
>    go get github.com/bsm/sarama-cluster
>
>    需要kafka 0.9及以上版本