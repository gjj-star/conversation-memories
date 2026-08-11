# Apache Flink

## 是什么

Apache Flink 是开源的**分布式流处理引擎**，实时计算领域的**事实标准**。由德国柏林工业大学的研究项目（Stratosphere）孵化，2014 年进入 Apache 孵化器。阿里巴巴是 Flink 的最大贡献者和使用者。

## 流处理 vs 批处理

| 维度 | 流处理（Flink 核心） | 批处理（Spark 核心） |
|------|----------------------|----------------------|
| 数据处理单位 | 一条一条（事件驱动） | 一批一批（定时触发） |
| 延迟 | **毫秒级** | 分钟级起步 |
| 数据范围 | 无界数据流 | 有界数据集 |
| 典型场景 | 实时大屏、风控、推荐 | 日报、离线报表、ETL |

Flink 的口号：**批处理是流处理的子集**——把有界数据看成"会结束的流"，流批一体。

## 核心概念

### Source → Transformation → Sink

```
数据源（Source）        处理逻辑（Transformation）      输出（Sink）
Kafka Topic    →   过滤/聚合/关联/开窗   →   Hologres / MySQL / Kafka / 大屏
MySQL CDC      →   实时计算指标         →   Elasticsearch / Redis
日志文件        →   清洗 + 结构化        →   数据湖（Iceberg/Hudi）
```

### 窗口机制

流处理中，"最近 5 分钟"是一个窗口——数据需要按时间切分再聚合。

| 窗口类型 | 行为 | 例子 |
|----------|------|------|
| **滚动窗口**（Tumbling） | 固定大小，不重叠 | 每 5 分钟统计一次 |
| **滑动窗口**（Sliding） | 固定大小，有重叠 | 每 1 分钟统计最近 5 分钟 |
| **会话窗口**（Session） | 按活跃间隔划分 | 用户从打开到离开 App 为一个会话 |

### 状态与 Checkpoint

- **状态（State）**：Flink 记住之前处理过的数据（比如当前计数、累加值）
- **Checkpoint**：定期把状态快照保存到持久化存储
- **精确一次语义（Exactly-Once）**：即使机器挂了，恢复后不会丢数据也不会重复计算

## 实时数仓典型链路

```
业务 DB（MySQL）──CDC──→ Kafka ──→ Flink（实时计算）──→ Hologres（实时存储）
                                                      ↓
                                                  大屏/Superset（实时展现）
```

用户下单 → 1 秒内出现在实时大屏上。这就是 Flink 的价值。

## Flink SQL

不用写 Java/Scala，用标准 SQL 写流计算：

```sql
-- 每 5 分钟统计各品类 GMV，滚动窗口
SELECT
  TUMBLE_START(order_time, INTERVAL '5' MINUTE) AS window_start,
  category,
  SUM(amount) AS gmv
FROM orders
GROUP BY TUMBLE(order_time, INTERVAL '5' MINUTE), category;
```

大大降低了流计算的门槛。

## 一句话总结

Flink = 实时流计算的王者。毫秒级延迟 + 精确一次语义 + 流批一体。阿里是最大推手，Flink SQL 让不会写 Java 的分析师也能玩实时计算。
