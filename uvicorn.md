# Uvicorn

## 是什么

Uvicorn 是基于 **ASGI**（Asynchronous Server Gateway Interface）协议的 Python 高性能异步 Web 服务器，是 **FastAPI 官方推荐的服务器**。

名字由来：Uvicorn = UV（ultraviolet，紫外线）+ Uni**corn**（独角兽），寓意"闪电般快"。

## 服务器 vs 框架

| 层次 | 代表 | 职责 |
|------|------|------|
| **Web 框架** | FastAPI、Django、Flask | 定义路由、处理业务逻辑、返回响应 |
| **Web 服务器** | Uvicorn、Gunicorn | 接收 HTTP 请求、管理连接、调度到框架 |
| **协议接口** | ASGI、WSGI | 框架和服务器之间的通信标准 |

**类比**：FastAPI 是餐厅的厨师（做菜），Uvicorn 是服务员（接单上菜），ASGI 是服务员和厨师之间的点菜系统。

## ASGI vs WSGI

| 维度 | WSGI（旧标准） | ASGI（新标准） |
|------|---------------|---------------|
| 并发模型 | 同步（一个请求占一个线程） | 异步（`async/await`） |
| WebSocket | ❌ 不支持 | ✅ 原生支持 |
| HTTP/2 | ❌ | ✅ |
| 代表服务器 | Gunicorn（默认）、uWSGI | **Uvicorn**、Daphne、Hypercorn |
| 代表框架 | Flask、Django（旧版） | FastAPI、Django Channels |

## 为什么 Uvicorn 这么快

三层高性能底层：

1. **asyncio**：Python 标准库的异步 I/O，用协程而非线程处理并发
2. **uvloop**：用 Cython 重写的 asyncio 事件循环，替代标准库的事件循环（性能逼近 Node.js/Go）
3. **httptools**：用 C 写的 HTTP 解析器，解析请求的速度极快

## 使用方式

### 开发环境
```bash
uvicorn main:app --reload   # 代码改了自动重启
```

### 生产环境（Gunicorn + Uvicorn workers）
```bash
gunicorn main:app -w 4 -k uvicorn.workers.UvicornWorker
# Gunicorn 管理进程（启动多个 worker、挂了重启）
# Uvicorn 在每个 worker 里处理异步请求
```

## 一句话总结

Uvicorn = Python 异步 Web 服务器中的性能标杆，FastAPI 御用。开发时一条命令跑起来，生产时配合 Gunicorn 做进程管理。
