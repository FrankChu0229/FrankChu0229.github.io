title: NoSQL Summary
date: 2017-12-18 15:25:39
tags: [notes, summary, NoSQL]
description: NoSQL Summary。
---

# NoSQL Summary

NoSQL指 not only SQL, 主要包括：

### **Key-value: Redis, 分布式：Codis** 

以键值对的方式进行存储，其内部通常用哈希表的结构来对数据进行存储。在使用时，用户只需要根据key来访问即可。优点：对单条数据的增删改查非常迅速；缺点：数据库不知道对于数据本身的任何信息，往往需要对数据库中的数据进行遍历，**不支持索引**。**Key-value型数据库通常在服务中作为缓存端使用**，例如可作为MYSQL的缓存端使用，可使得服务性能提高10+倍。

### **Document-based: mongoDB** 

与Key-value数据库不同，Document-based 数据库中存储的不再是字符串，而是像JSON, XML等具有特定格式的文档。这些文档可以记录键值对，数组，或者是内嵌的文档。**Document-based**数据库常常支持索引，因此**Document-based**数据库既保留了key-value型数据库的便利，又在查询上借助索引可以达到不错的性能。

### **Column-based: cassandra, Hbase**

按照列来在数据文件中记录数据，以便获得更好的遍历和请求效率，但并不是所有的数据都用列来存储，一般只有需要请求的数据会用列来存储。

### **Graph-based: neo4j, janus graph, dgraph**


## Reference

- https://www.cnblogs.com/loveis715/p/5299495.html
- https://www.cnblogs.com/aspwebchh/p/6652855.html

---