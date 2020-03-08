# Docker 运维操作

1. 如何查看 docker 容器对应宿主机 pid ?

方法一: `cat /sys/fs/cgroup/memory/docker/{dockerID}/cgroup.procs`

方法二: `docker top ${dockerID}`
