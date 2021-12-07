---
title: MySQL、jdbcType、Java Type之间的映射关系
author: BeiyanLuansheng
date: 2021-12-01 22:30:06 +0800
categories: [技术积累, Mybatis]
tags: [Mybatis, jdbcType]
math: true
mermaid: true
---

| MySQL Type         | JdbcType      | Java Type                  |
| ------------------ | ------------- | -------------------------- |
| CHAR               | CHAR          | String                     |
| VARCHAR            | VARCHAR       | String                     |
| LONG VARCHAR       | LONGVARCHAR   | String                     |
| NUMERIC            | NUMERIC       | java.math.BigDecimal       |
| DECIMAL            | DECIMAL       | java.math.BigDecimal       |
| BIT                | BIT           | boolean                    |
| BOOLEAN            | BOOLEAN       | boolean                    |
| TINYINT            | TINYINT       | byte                       |
| SMALLINT           | SMALLINT      | short                      |
| INTEGER            | INTEGER       | int                        |
| BIGINT             | BIGINT        | long                       |
| REAL               | REAL          | float                      |
| FLOAT              | FLOAT         | double                     |
| DOUBLE             | DOUBLE        | double                     |
|                    | BINARY        | byte[]                     |
|                    | VARBINARY     | byte[]                     |
|                    | LONGVARBINARY | byte[]                     |
| DATE               | DATE          | java.sql.Date              |
| TIME               | TIME          | java.sql.Time              |
| TIMESTAMP/DATETIME | TIMESTAMP     | java.sql.Timestamp         |
| CLOB               | CLOB          | Text                       |
| BLOB               | BLOB          | Blob                       |
|                    | ARRAY         | Array                      |
|                    | DISTINCT      | mapping of underlying type |
|                    | STRUCT        | Struct                     |
|                    | REF           | Ref                        |
|                    | DATALINK      | java.net.URL               |



参考：

- [http://www.mybatis.org/mybatis-3/apidocs/reference/org/apache/ibatis/type/JdbcType.html](http://www.mybatis.org/mybatis-3/apidocs/reference/org/apache/ibatis/type/JdbcType.html)

- [https://blog.csdn.net/loongshawn/article/details/50496460](https://blog.csdn.net/loongshawn/article/details/50496460)
