## 整体技术特征

1. 开源、免费、跨平台
2. 和Linux深度结合
3. 入门学习成本低
4. 社区庞大 生态完善
5. 人才储备充足
6. 版本迭代 越来越好

linuxforum chinaunix

## 最大化利用CPU资源

* 升级到新版本
* 关闭节能模式（bios 关闭CPU自动降频）
* 不跑“大”SQL（影响很多行）
* 不跑“大”事务（长时间不能提交）
* **高频SQL务必要有索引**

## 内存使用优化

* 按需设置全局的session级buffer  5.7+ 可以动态调整buffer pool
* 使用更多的物理内存
* 禁用query cache
* 不用MyISAM（存储引擎）表
* 关闭NUMA
* 关闭SWAP
* 关注内存泄漏及OOM风险

free -h

如果available远小于 total - used，大概率发生了内存泄漏

## 提高io效率

* 加大物理内存
* 使用更好的io设备
* 利用索引减少随机io
* 区分随机和顺序io
* 尽量将随机io变成顺序

## MySQL使用禁区

* 宽表 <=500B
* 大SQL 5000条（加锁长期不释放）
* 大事务 
* 全文模糊搜索
* 没自增主键
* 全单列索引
* 用MyISAM而非InnoDB



