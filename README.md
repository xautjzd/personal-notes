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
