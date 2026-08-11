# Embedding、Rerank、LLM 三种模型对比

## 一句话区分

| 模型类型 | 角色 | 一句话 |
|----------|------|--------|
| **Embedding** | 向量化 / 粗召回 | "把文本变成向量，从海量候选中快速捞出可能相关的 TOP-K" |
| **Rerank** | 精排 | "把粗召回的几十条结果按相关性重新打分排序，选出最相关的" |
| **LLM** | 生成 / 理解 | "基于最终上下文做阅读理解、总结、对话生成" |

## 技术架构对比

| 维度 | Embedding | Rerank | LLM |
|------|-----------|--------|-----|
| 架构 | 双塔（Bi-Encoder）：Query 和 Doc 各一个塔，独立编码后余弦相似度匹配 | 单塔（Cross-Encoder）：Query + Doc 拼接后一起输入 | Decoder-only（GPT 式）：自回归逐 token 生成 |
| 推理速度 | 极快（向量检索，ANN 索引） | 中等（每条候选都要过一遍模型） | 慢（逐 token 生成） |
| 典型大小 | 0.1B-1B 参数 | 0.3B-3B 参数 | 7B-2000B+ 参数 |
| 输入长度 | 短文本为主（512 token） | 中等（512-8K token） | 长文本（128K-1M token） |
| 输出 | 固定维度向量 | 相关性分数 | 自然语言文本 |

## RAG 三层 Pipeline 协作

```
用户问题
  ↓
① Embedding：将问题向量化，在向量库中 ANN 检索 → 召回 TOP-50~100 候选文档
  ↓
② Rerank：对 TOP-50 逐条精排打分 → 筛选 TOP-5~10 真正相关的
  ↓
③ LLM：将 TOP-5 拼接成 Prompt → 阅读理解 + 生成回答
```

**为什么不能只用一步？**
- 只用 Embedding：粗召回精度不够，TOP-1 可能不相关
- 只用 Rerank：不能在海量数据上逐一跑（太慢太贵），需要 Embedding 先缩小范围
- 只用 LLM：无法塞入所有知识，需要前两步筛选出最相关上下文

## 代表模型

| 类型 | 开源代表 | 商业代表 |
|------|----------|----------|
| Embedding | BGE-M3、Multilingual-E5、Jina Embeddings | OpenAI text-embedding-3、Cohere Embed |
| Rerank | BGE-Reranker-v2、Jina Reranker | Cohere Rerank、Voyage Rerank |
| LLM | Qwen、DeepSeek、Llama | GPT-4o、Claude |

## 一句话总结

Embedding 干"海选"（快但粗），Rerank 干"复赛"（慢但准），LLM 干"决赛"（最慢但最聪明）。三者不是竞争关系，是分工协作的 Pipeline。
