# SSE 流式解析

## 是什么

SSE（Server-Sent Events）是一种基于 HTTP 的服务端向客户端**单向推送**技术。与 WebSocket 不同，SSE 只支持服务端 → 客户端，不支持客户端回推。

**数据格式**：纯文本流，由 `event:`、`id:`、`data:` 字段行 + 空行（`\n\n`）分隔每条消息。

```
event: update
id: 42
data: {"user": "alice", "status": "online"}

event: message
data: hello
data: world

```

## 流式解析的核心挑战

HTTP 响应是分 chunk 到达的，一个 SSE 消息可能跨多个 chunk：

```
Chunk 1: "data: {\"token\": \"hel"
Chunk 2: "lo\", \"index\": 5}\n\n"
```

解析器需要**缓冲区管理**：

1. 收到 chunk → 追加到缓冲区
2. 在缓冲区中查找 `\n\n`（消息分隔符）
3. 完整消息 → 解析各字段行 → 清空已处理部分
4. 不完整 → 保留在缓冲区等待下一个 chunk

### 常见截断情况

| 情况 | 处理方式 |
|------|----------|
| 不完整行（`data: {"to`） | 保留，等 `\n` 到来 |
| 不完整消息（行完了但缺 `\n\n`） | 保留整条，等双换行 |
| 跨 chunk 的 JSON | 累积到完整 JSON 再 parse |

## LLM 场景下的 SSE

当前主流 LLM API（OpenAI、Anthropic 等）都用 SSE 实现流式输出（"打字机效果"）：

```
data: {"choices":[{"delta":{"content":"你好"}}],"id":"chatcmpl-xxx"}

data: {"choices":[{"delta":{"content":"，"}}],"id":"chatcmpl-xxx"}

data: {"choices":[{"delta":{"content":"世界"}}],"id":"chatcmpl-xxx"}

data: [DONE]
```

每个 `data:` 帧承载一个 token，客户端逐个渲染即可实现逐字输出效果。`data: [DONE]` 是标准结束标记。

## SSE vs WebSocket

| 维度 | SSE | WebSocket |
|------|-----|-----------|
| 方向 | 服务端 → 客户端（单向） | 双向 |
| 协议 | HTTP（普通请求即可） | 独立协议 `ws://` |
| 重连 | 浏览器内置自动重连 | 需手动实现 |
| 复杂度 | 低 | 高（心跳、帧类型等） |
| 适用场景 | 实时推送、LLM 流式输出 | 聊天、游戏、协同编辑 |

## 一句话总结

SSE 流式解析 = 在 HTTP 响应流上逐 chunk 读取 `data:` 文本行，用缓冲区处理跨 chunk 截断，拼出完整消息后解析。LLM 的"逐字输出"就是靠它实现的。
