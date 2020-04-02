# 网络排查 tips

### 查看容器与主机 vethpair

方法一:

1. 进入到容器，执行`cat /sys/class/net/eth0/iflink` 查看 index。
2. 在宿主机执行 `ip link show|grep <index>`, 其中 index 为上面查到的值。

方法二:

1. 在容器里安装 ethtools, 然后执行 `ethtool -S eth0` 查看 index。
2. 与方法一相同。
