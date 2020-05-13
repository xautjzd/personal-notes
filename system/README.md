# System Design

## System architecture pattern

### The Layered Architectural Pattern
### The Microkernal Architectural Pattern
### The CQRS Architectural Pattern
### The Event Driven Architectural Pattern
### The Microservices Architectural Pattern
### The Client-server Architectural Pattern

References:

1. https://blog.ndepend.com/software-architecture-5-patterns-you-need-know/
2. https://medium.com/ios-expert-series-or-interview-series/software-architectural-patterns-design-structures-c5692fe8affc

## 限流设计

## 熔断设计

## 降级设计

### 目的

在有限资源情况下，面对突发流量递增，系统压力过大，为了防止系统崩溃，保证系统可用性，需要对系统进行降级

### 解决思路

1. 降低一致性：强一致性变为最终一致性
2. 停止次要功能，保证核心功能可用
3. 简化功能, 减少非必要数据显示

降级包含 功能降级 & 服务降级两大类。

功能降级：

1. 通过降级开关控制功能是否可用，一般也页面和按钮
2. 简化业务操作流程，快速完成业务操作

服务降级：

1. 读降级: 降级前先读缓存，缓存中不存在再读数据库；降级后读缓存，缓存不存在，返回默认值，不再读取数据库
2. 写降级: 降级前同步写数据库；降级后先写缓存，再异步同步至数据库
3. 服务调用降级: 降级前服务间通过 mq 通信，mq 消息堆积或 mq 宕机，降级为 http 调用


## 异步通讯

### 为什么要选择异步通讯

同步调用存在以下几个问题：

1. 同步调用需要被调用方吞吐不低于调用方吞吐，否则会拖垮调用方(漏桶效应)
2. 同步调用只能一对一，很难一对多
3. 同步调用会导致调用方一直等待直到调用完成，对于高并发场景比较消耗资源

### 异步通讯方式

1. 请求响应式(要么轮询，要么注册回调)
2. 发布订阅(发送方姜消息/数据发送至接收方订阅的队列，接收方从队列中获取数据)
3. Broker 形式(发送方将数据发送至 Broker，接收方订阅 Broker)

### 异步通讯缺点

1. 业务流程实现相对复杂
2. 事务处理相对复杂

## 分布式锁

1. [Redis Redlock](https://redis.io/topics/distlock)
2. [How to do distributed locking](https://martin.kleppmann.com/2016/02/08/how-to-do-distributed-locking.html)
3. [Is Redlock safe?](http://antirez.com/news/101)

## 分布式事务

### 刚性事务

两阶段提交(2PC)

### 柔性事务

- BASE
- 重试 & 幂等
- 事务消息
- 事务补偿(TCC)

[浅谈事务和一致性：刚性or柔性](https://juejin.im/post/5aa8b8636fb9a028c67567c6#heading-18)
