---
name: auto-summarize-directive
description: 指示：当对话中产出参考型知识时，自动写入 memory/ 并推送
metadata: 
  node_type: memory
  type: feedback
  originSessionId: ccaa9fff-80e5-4190-ab19-dc67c8a34ec5
---

## 自动知识捕获规则

当对话中出现以下情况时，自动执行：

### 触发条件

用户问了一个**事实/概念/方法论**类的问题，且我产出了有结构的回答。典型场景：

- "XXX 是什么" → 保存定义、特点、适用场景
- "XXX 有什么优势和劣势" → 保存对比表
- "XXX 和 YYY 的关系" → 保存关系分析
- "XXX 领域发展到什么程度" → 保存前沿总结
- "XXX 的原理是什么" → 保存概念解释

不属于此范围：项目代码操作、单次工具调用、闲聊、已有回答的简单确认。

### 执行动作

1. 在回答完用户问题后，将核心知识提炼为 memory 文件，保存到 `~/.claude/projects/D--CC/memory/<topic-slug>.md`
2. 格式：frontmatter（name/description/type: reference）+ Markdown 正文
3. 更新 `MEMORY.md` 索引
4. Stop hook 会自动 git commit + push 到 https://github.com/gjj-star/conversation-memories.git

### 不重复写入

如果已有同名 memory 文件且内容覆盖了当前知识，跳过。如果新内容是对现有知识的补充（新发现、新观点），编辑已有文件追加。

**Why:** 对话中产出的知识如果不记录，下次对话就会丢失，需要重新推导。自动捕获让知识库持续成长。

**How to apply:** 每次回答完参考型问题后，检查 memory/ 目录是否已有覆盖，无则创建，有且不足则补充。
