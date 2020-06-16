# 内存管理

## 工具

- free
- pmap

## 写缓存

- 不缓存(nowrite)
- 写透缓存(write-through cache)
- 回写(write-back)

## 缓存回收

### 何时回收？

1. 当空闲的内存低于某一个值时（查看/proc/sys/vm/dirty_background_ration），会把1024个dirty page写回磁盘中，直到内存回到空闲状态。

2. 当dirty page超过一定的比例时（查看/proc/sys/vm/dirty_ratio），内核会启动pdflush线程把超过的那一部分启写入磁盘中。

3. 当dirty page超过了一定的时间（查看/proc/sys/vm/dirty_expire_centisecs），才会被写入磁盘中。

4. 当用户程序调用了sync() 和 fsync()系统调用。

### 回收策略

1. LRU
2. 双链策略
