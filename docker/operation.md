# Docker 运维操作

1. 如何查看 docker 容器对应宿主机 pid ?

方法一: `cat /sys/fs/cgroup/memory/docker/{dockerID}/cgroup.procs`

方法二: `docker top ${dockerID}`

2. how to find host/container veth pair ?

**solution 1**

- exec `cat /sys/class/net/eth0/iflink` in container
- exec `grep -R <container iflink value> /sys/class/net/*/ifindex`

**solution 2**

 in container, run: `ip link show eth0`, result will be like this:

```
116: eth0@if117: <BROADCAST,MULTICAST,UP,LOWER_UP,M-DOWN> mtu 1500 qdisc noqueue state UP
    link/ether 02:42:0a:00:04:08 brd ff:ff:ff:ff:ff:ff
```

in host, run: `ip link show|grep 117`
