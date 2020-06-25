Elasticsearch设计理念就是分布式搜索引擎，底层是lucene.
核心思想：在多台机器上启动多个es进程实例，组成es集群。es中存储数据的基本单位是索引，多有订单数据都写到索引里面去，一个索引差不多相当于mysql里的一张表。
基本结构：index->type->mapping->field
   index: mysql里的一张表
   type: 允许在一个index里存储多种类型的数据，这样可以减少index的数量。在使用时，向每个文档加入type字段，在指定type搜索时就会被用于过滤。好处是，搜索一个index下的多个type,和只搜索一个index下的多个
   type相比没有额外的开销，需要合并的分片数量一致。
   mapping: 这个type的表结构定义，定义了这个type中的每个字段名称，字段是什么类型，然后这个字段的各种配置。
   document: index里的一个type里面写的一条数据，一条document代表mysql中某个表里的一行数据，
   field: 每个document有多个field,每个field就代表这个document中的一个字段的值
索引Index可以拆分成多个shard,每个shard存储部分数据。这个shard数据有多个备份，其中primary shard负责写入数据，
replica shard 会自动同步primary shard写入的数据。如果某个机器宕机了，重新选取primary shard。写只能往primary shard写，读没要求
