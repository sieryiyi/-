https://www.bilibili.com/video/BV1sV4y147Jz/?spm_id_from=333.788&vd_source=07999cb1d010fa9357b6650a0ee711a7

高可用的缓存数据库

### 缓存击穿、缓存穿透、缓存雪崩

### RDB

周期性备份是分钟级别的

### AOF持久化

  -------------------------------------**AOF生成快、加载慢，RDB相反（？）**-------------------------

  - 把所有写入命令都写入到AOF文件
  - 
  - 把要写入的命令，先写入到临时缓冲区aof_buf，再择机写入AOF文件（硬盘里）
  - 
  - 随着AOF文件越来越大，需要进行压缩：AOF重写，只写入最终数据状态
  - 
  - 用一个子进程去专门进行重写操作
  - 
  - 再用一个缓冲区：aof重写缓冲区，在重写的过程中，记录来的新的命令，等重写完成，再把这些命令也写入AOF文件
  
### Redis 哨兵

  - 负责调度主节点、从节点

  - 如果主节点掉线，调度某一个从节点充当主节点

  - 为了及时获得和更新主从节点信息，每隔10秒用info命令向主节点询问都有哪些从节点

  - 每隔10s用ping询问子节点是否掉线（多个哨兵共同确定掉线，才是掉线）

  - 如果主节点掉线，启动故障转移

  - 先选一个新的主节点 → 让其他子节点从新的主节点中同步数据 → 将掉线的主节点改为从节点

  - 选新的主节点的思路：优先级越高的；复制偏移量越大的；断开主节点时间越短的


  