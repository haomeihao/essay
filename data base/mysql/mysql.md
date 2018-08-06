#### Mysql

Mysql事务隔离级别

隔离级别   脏读(Dirty Read) 不可重复读(NonRepeatable Read) 幻读(Phantom Read) 
未提交读（Read uncommitted）  可能   可能   可能
已提交读（Read committed）  不可能   可能   可能
可重复读（Repeatable read） 不可能 不可能   可能
可串行化（Serializable ）   不可能 不可能 不可能

脏读：一个事务中读取了另一个事务中未提交的数据
不可重复读：一个事务中两次读取由于另一个事务修改已提交的数据导致不一致
可重复读：在同一个事务内的两次查询都是事务开始时刻一致的。
幻读：一个事务，没有读取到，但不知不觉更新了另一个事务新增的数据。

