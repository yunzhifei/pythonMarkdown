# MySQL index 索引组织表

1. innodb 是主键组织表
   1. 有指定主键，就指定主键没有的就用定义的第一个非空唯一索引，如果没有非空唯一索引就自动创建一个6字节的ID
   2. ![image-20191218110241387](../images/image-20191218110241387.png)
2. 