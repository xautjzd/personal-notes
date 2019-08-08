# Docker 数据持久化

### 容器内存储数据弊端

- 容器退出，数据丢失
- 难以将数据共享给容器外的其他进程
- 数据难以迁移到其他地方

## 数据持久化三种方式

- volumes
- bind mounts
- tmpfs mount(Linux)/named pipe(Windows)

## Reference

[https://docs.docker.com/storage/](https://docs.docker.com/storage/)
