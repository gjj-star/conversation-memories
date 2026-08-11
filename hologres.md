# Hologres

## 是什么

Hologres 是**阿里云自研的一站式实时交互式分析引擎**，定位 HSAP（Hybrid Serving/Analytical Processing）——同时支撑实时写入、实时分析和在线服务。

它**完全兼容 PostgreSQL 协议**，意味着你不需要学新语法，用 PG SQL 就能操作。

## 核心特点

| 特点 | 说明 |
|------|------|
| **HSAP 一体** | 同一张表既能实时写入、又能 OLAP 分析、还能高并发点查，不需要 T+1 的"昨天的数据" |
| **PG 协议兼容** | SQL 语法、驱动、工具全部复用 PostgreSQL 生态 |
| **阿里生态融合** | 与 Flink（实时计算）、MaxCompute（离线数仓）、DataWorks（数据开发）深度集成 |
| **存储计算分离** | 存储和计算独立扩缩容，成本更灵活 |

## Hologres vs 同类产品

| 维度 | Hologres | ClickHouse | TiDB/TiFlash |
|------|----------|------------|--------------|
| OLAP 分析 | ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐（最强的列存分析） | ⭐⭐⭐ |
| 高并发点查 | ⭐⭐⭐⭐⭐ | ⭐⭐（不擅长） | ⭐⭐⭐⭐ |
| 实时写入 | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐ |
| PG 兼容 | ✅ 完全 | ❌ | ✅（TiDB 兼容 MySQL） |
| 阿里云集成 | ✅ 原生 | 需自建 | ❌ |
| 定位 | 实时数仓 + 在线服务 | 纯粹 OLAP | HTAP 分布式数据库 |

## 一句话概括

> **PostgreSQL 协议 + ClickHouse 级 OLAP + Redis 级点查并发 + Flink 实时写入** = Hologres

## 为什么需要 Hologres

传统架构的痛点：
```
实时写入 → Kafka
离线分析 → MaxCompute/Hive（T+1，昨天才能看）
在线查询 → Redis/MySQL（数据结构受限）

三条线各自维护，数据不一致，时效性差。
```

Hologres 做掉这一切：
```
Flink 实时写入 → Hologres → 大屏实时刷新（1 秒延迟）
                           → API 直接查（高并发点查）
                           → BI 工具对接（Superset/Tableau）
```

## 一句话总结

Hologres = 阿里云的"实时数仓一体机"。用 PG SQL，既能做 ClickHouse 式的实时分析，又能做 Redis 式的高并发点查。如果全在阿里云上，它是实时数仓的最省心选择。
