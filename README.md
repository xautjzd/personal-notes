# 个人笔记

## 操作系统

### 1. Linux内核

[内核参数](os/kernal.md)

## 抓包工具

tcpdump常用参数:

- `-A`
- `-B`
- `-c`
- `-i`
- `-nn`
- `-r`
- `-w`
- `-x`

只抓取三次握手的第一轮SYN:

```
#tcpdump 'tcp[tcpflags] == tcp-syn' -x 
```

参考资料: [https://serverfault.com/questions/217605/how-to-capture-ack-or-syn-packets-by-tcpdump](https://serverfault.com/questions/217605/how-to-capture-ack-or-syn-packets-by-tcpdump)

## 网络协议

### TCP协议

[TCP协议](network/tcp.md)

### IP协议

### ICMP协议

[ICMP协议](network/icmp.md)

### Mac协议

## Docker & Kubernetes

### Storage

- [Docker 存储](docker/storage.md)
- [Kubernetes Volumes](kubernetes/volume.md)

### Network

### Kubernetes Informer

- [扩展kubernetes API](https://mp.weixin.qq.com/s?__biz=MzU1OTAzNzc5MQ==&mid=2247484052&idx=1&sn=cec9f4a1ee0d21c5b2c51bd147b8af59&chksm=fc1c2ea4cb6ba7b283eef5ac4a45985437c648361831bc3e6dd5f38053be1968b3389386e415&scene=21#wechat_redirect)
- [Kubernetes Informer 使用](https://www.jianshu.com/p/1e2e686fe363?isappinstalled=0)

## 数据库

### 学习资源

- [SIGMOD](https://sigmod.org/)
- [VLDB](http://vldb.org/)
- [ICDE](https://www.icde.org/)
- [USENIX ATC](https://www.usenix.org/conference/atc19)
- [SOSP](http://sosp.org/)
- [OSDI](https://www.usenix.org/conference/osdi18)


## 系统设计

- [system design](https://github.com/donnemartin/system-design-primer)

## 其他

- [http://www.cburch.com/cs/](http://www.cburch.com/cs/)
- [compilers](https://web.stanford.edu/class/cs143/index2018.html)
- [Computer Science](https://cs.stanford.edu/directory/faculty)
