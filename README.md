# lakefs 调研

## 软件设计

![](./README/arch.png)

数据存储:  s3

metadata 存储: PostgreSQL

本身无状态即可 轻松大规模部署


### Metadata

Lakefs 中的提交为不可变提交  

commits 存储在 LSM 数据结构中

TODO： kv数据当前是否存在在pgsql中

lakefs 团队未来的计划: 

> 我们计划通过嵌入Raft来消除对 RDBMS 的需求，以在一组机器上复制这些写入，并将数据本身存储在 RocksDB 中。为了方便操作，复制的 RocksDB 数据库会定期快照到底层对象存储。

### 服务组成

- s3 api gateway
- open api
- s3 storage adapter (标准interface 适配不同底层存储结构)
- web ui
- auth server


### 基础概念

- Repositories 存储库

类似 namespace 将 对象、分支和提交组合在一起 相当于 Bucket

- Branches  分支

概念参考 Git 分支 ， 新开一个分支 实际上为 创建当前 存储库的一致性快照 

- Commits

提交是不可变的“检查点”，包含在给定时间点的存储库的**完整快照**

- Objects

文件即对象
