# DDos

攻击原理：

1. 耗尽网络带宽
2. 耗尽操作系统资源
3. 消耗应用程序运行资源

## 相关工具

- hping3
- sar
- tcpdump
- netstat

## 模拟 DDos 攻击

`hping3 -S -p 80 -i u10 192.168.1.10`

其中 `-S` 参数表示设置 TCP 协议的 SYN，`-p` 表示目的端口为80，`-i u10` 表示每隔 10 us 发送一个数据包

`netstat -n -p|grep SYN_REC`, 查看是否有大量的 `SYNC_RECV` 状态连接

## 缓解 DDos 攻击

### 限制 sync 包速率

```
# 限制syn并发数为每秒1次
$ iptables -A INPUT -p tcp --syn -m limit --limit 1/s -j ACCEPT

# 限制单个IP在60秒新建立的连接数为10
$ iptables -I INPUT -p tcp --dport 80 --syn -m recent --name SYN_FLOOD --update --seconds 60 --hitcount 10 -j REJECT
```

### 增大 sync backlog

`sysctl -w net.ipv4.tcp_max_syn_backlog=4096`

### 关闭 sync 失败重试

另外，连接每个 SYN_RECV 时，如果失败的话，内核还会自动重试，并且默认的重试次数是 5 次, 关闭重试

`sysctl -w net.ipv4.tcp_synack_retries=1`

### TCP SYN Cookies

`sysctl -w net.ipv4.tcp_syncookies=1`

开启了 SYN Cookies，没有必要设置 `net.ipv4.tcp_max_syn_backlog` 了

### 其他解决方法

1. WAF(Web Applciation Fileware)
2. CDN
3. 购买专业的流量清洗设备和网络防火墙
4. 内核调优、DPDK、XDP 等
