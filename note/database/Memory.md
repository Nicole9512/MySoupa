Memory
=================

# 一、简介
* MEMORY 存储引擎使用存在内存中的内容来创建表。每个 MEMORY 表只实际对应一个磁盘文件，格式是.frm。MEMORY 类型的表访问非常得快，因为它的数据是放在内存中的，并且默认使用 HASH 索引，但是一旦服务关闭，表中的数据就会丢失掉。

# 二、使用
* 在启动 MySQL 服务的时候使用--init-file 选项，把 INSERT INTO ... SELECT 或 LOAD DATA INFILE 这样的语句放入这个文件中，就可以在服务启动时从持久稳固的数据源装载表。
* 服务器需要足够内存来维持所有在同一时间使用的 MEMORY 表，当不再需要 MEMORY 表的内容之时，要释放被 MEMORY 表使用的内存，应该执行 DELETE FROM 或 TRUNCATE TABLE， 或者整个地删除表(使用 DROP TABLE 操作)。
* 每个 MEMORY 表中可以放置的数据量的大小，受到 max_heap_table_size 系统变量的约束，这个系统变量的初始值是 16MB，可以按照需要加大。此外，在定义 MEMORY 表的时候，可以通过 MAX_ROWS 子句指定表的最大行数。
* MEMORY 类型的存储引擎主要用在那些内容变化不频繁的代码表，或者作为统计操作的中间结果表，便于高效地对中间结果进行分析并得到最终的统计结果。对 MEMORY 存储引擎的表进行更新操作要谨慎，因为数据并没有实际写入到磁盘中，所以一定要对下次重新 启动服务后如何获得这些修改后的数据有所考虑。