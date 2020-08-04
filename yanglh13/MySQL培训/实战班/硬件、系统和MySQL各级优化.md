## 硬件层优化

* 关闭节能模式
* 关闭NUMA（swap） bios|grub|numactl
* 阵列卡选择FORCE WB策略
* 阵列里挂载更多磁盘
* 阵列卡配CACHE、BBU
* 选用更好的IO设备

## 系统层优化

* 调整ioscheduler、noop、deadline
* 调整文件系统、xfs
* 关闭大页、hugepage、OOM
* 考虑关闭SWAP
* 修改脏页比例

## MySQL关键状态

show global status;

show processlist;