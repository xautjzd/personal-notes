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

TCP协议头格式:

```
 0                   1                   2                   3
    0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |          Source Port          |       Destination Port        |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |                        Sequence Number                        |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |                    Acknowledgment Number                      |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |  Data |           |U|A|P|R|S|F|                               |
   | Offset| Reserved  |R|C|S|S|Y|I|            Window             |
   |       |           |G|K|H|T|N|N|                               |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |           Checksum            |         Urgent Pointer        |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |                    Options                    |    Padding    |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |                             data                              |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```

其中`Data Offset`为32个字节头部大小, 占用4bit,故TCP头最大为: (2^4 - 1) * 4 = 60字节

TODO

- 标识符解释
- Options解释
- 抓包举例

参考资料: [https://tools.ietf.org/html/rfc793#section-3.1](https://tools.ietf.org/html/rfc793#section-3.1)

### IP协议

### Mac协议
