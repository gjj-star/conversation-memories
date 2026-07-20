---
name: ontology-concept
description: Ontology 双层含义：哲学本体论（存在之学）与计算机本体（形式化概念规范）、与知识图谱的关系、Palantir Ontology 与 FDE 的关联
metadata: 
  node_type: memory
  type: knowledge
  originSessionId: a664a991-04f8-4750-aa50-a32de0485b49
  modified: 2026-07-20T03:30:34.737Z
---

Ontology（本体论/本体）有两层同源含义：

**1. 哲学本体论（源头）**：形而上学分支，研究"存在"本身——什么存在、存在意味着什么、万物如何分类（实体/属性/关系/事件）。词源：希腊语 ontos（存在者）+ logos（学说）。代表：亚里士多德《范畴篇》、海德格尔《存在与时间》、蒯因（"存在即成为约束变量的值"）。

**2. 计算机/AI 本体（借用义）**：Gruber 1993 经典定义——"对概念化的显式规范"（an explicit specification of a conceptualization）。即把某领域的概念体系用机器可处理的方式显式定义出来，四大构件：类（Class）、实例（Instance）、关系（Relation）、约束（Axiom）。例：电商本体定义"用户/商品/订单"类、"购买"关系、"订单必须有买家"约束。

**与知识图谱的关系**：本体 = 图谱的 schema/骨架层（概念与规则），知识图谱 = 填入的实例数据。技术栈：RDF/RDFS、OWL、SPARQL、Protégé。应用：语义网、数据集成、生物医学（Gene Ontology）、企业数据建模。

**Palantir Ontology 与 FDE**：Palantir Foundry 的核心概念——把企业数据映射为业务对象（对象 + 属性 + 动作 Action），形成业务人员可操作的语义层。FDE（Palantir 首创的岗位，见 [[pm-vs-fde-comparison]]）的核心工作之一就是深入客户业务、构建和维护其 Ontology。AI 时代本体回热：为 LLM/Agent 提供业务语义层（RAG、text-to-SQL 语义层、agentic AI 的业务上下文）。

**一句话记忆**：本体 = 明确回答"这个领域/世界里有哪些东西、它们如何分类和关联"的形式化清单；哲学问的是整个世界，工程问的是某个业务域。
