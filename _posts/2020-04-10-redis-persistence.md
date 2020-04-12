---
layout: article
title: Redis - 持久化机制
tags: Redis
 
lang: zh-Hans
key: Redis_Persistence
pageview: true
toc: true
show_subscribe: false
---

Redis 是内存型数据库，一旦服务器进程退出，Redis 中的数据就会丢失，为了解决这一问题，Redis 提供了两种持久化的方案，将内存中的数据保存到磁盘中，避免数据丢失。

## 一、RDB

Redis 提供默认的 RDB 持久化功能，这一功能可以将 Redis 在内存中的数据保存到硬盘中，可以手动执行，也可以在 `redis.conf` 中配置成定期执行。

### 1. 手动执行

RDB 文件可以通过两个命令来生成：

- **SAVE**：阻塞 Redis 的服务器进程，直到 RDB 文件被创建完毕。
- **BGSAVE**：fork 一个子进程来创建 RDB 文件，记录此刻的数据，交给子进程去创建 RDB 文件，父进程继续处理父进程接收到的命令。

RDB 文件的载入通常是自动执行的，当启动 Redis 服务器时，自动加载 RDB 文件。

如果 Redis 开启了 AOF 持久化，Redis 会优先使用 AOF 文件来还原数据。

### 2. 配置执行

可以配置多条规则，因为 Redis 每个时段的读写请求是不均衡的，为了平衡性能与数据安全，可以自定义多个规则，只要满足一个规则，就会执行 `BGSAVE` 命令。

```
save 900 1		# 服务器在 900 秒之内被修改了 1 次
save 300 10		# 服务器在 300 秒之内被修改了 10 次
save 60 10000 # 服务器在 60 秒之内被修改了 10000 次
```

## 二、AOF

RDB 并不是完全可靠的，如果服务器突然宕机或者电源断了，那么最新的数据就会丢失。而 AOF 文件提供了一种更可靠的持久化方式。每当 Redis 接受到修改数据的命令时，就会把命令追加到 AOF 文件中，当重启 Redis 时，AOF 里的命令会被重新执行一次。

### 1. 开启方式

AOF 和 RDB 持久化方式可以同时启动并且无冲突，如果 AOF 开启，启动 Redis 时会加载 AOF 文件。

```
appendonly yes
```

### 2. 相关配置

`appendfsync` 配置会影响服务器多久完成一次命令的记录，默认使用 `everysec`：

- always：将缓存区的内容总是即时写到AOF文件中
- everysec：将缓存区的内容每隔一秒写入AOF文件中
- no：写入AOF文件中的操作由操作系统决定，一般而言为了提高效率，操作系统会等待缓存区被填满，才会开始同步数据到磁盘

### 3. 日志重写

随着写操作的不断增加，AOF 文件会越来越大。例如递增一个计数器 100 次，那么最终结果就是数据集里的计数器的值为最终的递增结果，但是 AOF 文件里却会把这 100 次操作完整的记录下来。而事实上要恢复这个记录，只需要 1 个命令就行了，也就是说 AOF 文件里那 100 条命令其实可以精简为 1 条。所以 Redis 支持这样一个功能：在不中断服务的情况下在后台重建 AOF 文件。

重写过程如下：

- fork 一个子进程
- 子进程把新的 AOF 写到一个临时文件里
- 主进程持续把新的变动写到内存里的 Buffer，同时也会把这些新的变动写到旧的 AOF 里，这样即使重写失败也能保证数据的安全
- 当子进程完成文件的重写后，主进程会获得一个信号，然后把内存里的 Buffer 追加到子进程生成的那个新 AOF 里

Redis 会记录上一次重写后的 AOF 文件大小，如果当前文件的大小超过记录大小的多少百分比，会触发重写；同时也要满足文件的大小大于某一指定值。

```
auto-aof-rewrite-percentage 100
auto-aof-rewrite-min-size 64mb
```

## 三、区别

- RDB 持久化，安全性较差，它是正常时期数据备份及主从数据同步的最佳手段，文件尺寸较小，恢复数据较快。
- AOF 更安全，可将数据及时同步到文件中，但需要较多的磁盘 IO、AOF 文件较大，文件数据回复相对较慢，也更完整。