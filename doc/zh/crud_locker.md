# CRUDLocker (增删改查锁定器)

## 概念

* Resource: 资源, 代表一个特定对象的字符串代号
* Action: 操作, 即增查改删(`create`, `retrieve`, `update`, `delete`)

时间相关(均以毫秒为单位):
* ttl: 生命期(TimeToLive)
* max_wait: 最长等待时间

内部机制
* lock: 锁定某个资源至某种操作状态
* mark: 在lock的过程中设置的更短周期的一个锁, 使得其他lock操作暂时被阻拦, 以防执行lock流程时资源状态改变.
* lru: 资源的Least Recently Used状态统计.

## 主要方法

### `lock_do(resource, action='retrieve', ttl=None, max_wait=None, func=None, func_args=[], func_kwargs={})`

锁定资源并执行操作.

如始终等不开别的锁, 则抛出`CRUDLocker.Timeout`异常.

### `lru_count()`

获取根据内部的lru容器估算的资源数.

### `lru_get_oldest(count=1)`

获取最老的一些资源名. 返回列表.

> hint: count为负数时, 表示终止于该负索引; 例如`count=-1`则意味着取出全部.