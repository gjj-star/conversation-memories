# PostgreSQL

## 是什么

PostgreSQL（简称 PG）是**全球最先进的开源关系型数据库（RDBMS）**。始于 1986 年加州大学伯克利分校的 POSTGRES 项目，30+ 年持续演进，目前由 PostgreSQL Global Development Group 维护。

**它不是"又一个 MySQL"**——PG 的功能深度和扩展性在关系型数据库中是顶级的。

## 核心特点

| 特点 | 说明 |
|------|------|
| **完整 SQL 标准** | 对 SQL 标准的兼容性在所有开源数据库中最高 |
| **ACID 事务** | 完整的事务支持，数据一致性可靠 |
| **MVCC（多版本并发控制）** | 读写不互相阻塞，高并发场景下表现优异 |
| **丰富数据类型** | JSONB、数组、几何、IP 地址、UUID、hstore（KV）、全文搜索 |
| **强大扩展体系** | PostGIS（地理空间）、pgvector（向量检索）、TimescaleDB（时序）、Citus（分布式） |
| **BSD 协议** | 完全免费，商用无限制，不像 MySQL 有 GPL 顾虑 |

## PostgreSQL vs MySQL

| 维度 | PostgreSQL | MySQL |
|------|------------|-------|
| SQL 标准兼容 | ⭐⭐⭐⭐⭐ | ⭐⭐⭐ |
| 复杂查询 | 强（CTE、窗口函数、LATERAL JOIN 等） | 中 |
| JSON 支持 | JSONB（二进制存储，支持索引） | JSON（文本存储，索引弱） |
| 扩展生态 | 极其丰富 | 较少 |
| 简单 CRUD | 够用 | 更简单直接 |
| 流行度 | 持续增长 | 仍然是第一（历史惯性） |
| 适合场景 | 复杂业务逻辑、数据分析、GIS | 简单 Web 应用、读写分离架构 |

## 为什么 Hologres 等其他数据库要兼容 PG 协议

PG 的 SQL 方言和协议（wire protocol）已成为**事实标准**：
- 生态工具丰富：pgAdmin、DBeaver、DataGrip 都能连
- 人才供给充足：懂 PG SQL 的人很多
- 迁移成本低：从 PG 迁到兼容 PG 的数据库几乎零改动

阿里云 Hologres、AWS Redshift、Google AlloyDB、CockroachDB 等都选择兼容 PG 协议。

## 一句���总结

PostgreSQL = 开源关系型数据库的"瑞士军刀"——ACID 事务、JSONB、PostGIS 地理计算、pgvector 向量检索一个都少不了。功能深度远超 MySQL，是复杂业务场景的首选。
