# DataMigrationTree
数据库数据迁移技术

<pre>
Mydumper
      Mydumper是一个针对MySQL和Drizzle的高性能多线程备份和恢复工具。
</pre>

![](https://i.imgur.com/fuVOY2r.png)

<pre>
Mydumper的主要特性
      1）并行执行，性能提升
      2）易于管理
      3）一致性，在所有的threads之间维护快照，提供master和slave日志的精确位置等
      5）管理型,支持PCRE
</pre>

<pre>
Mydumper的主要工作步骤：
      1）主线程flush tables with read lock，施加全局只读锁，以组织dml语句写入，保证
         数据的一致性
      2）读取当前时间点的二进制日志文件名和日志写入的位置补并记录metadata文件中，以供恢复
         使用。
      3）start transaction with consistent snapshot，开启读一致性事务
      5）启用n个dump线程导出表和表结构
      6）备份非事务类型的表
      7）主线程unlock table，备份完成非事务类型的表之后，释放全局只读锁
      8）基于事务dump innodb tables
      9）事务结束
</pre>