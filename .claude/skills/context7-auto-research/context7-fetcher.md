---
name: context7-fetcher
description: 执行 Context7 API 调用的独立子任务（内部使用）
version: 1.0.0
author: BenedictKing
allowed-tools: Bash
context: fork
---

# Context7 Fetcher 子技能

> **注意**：这是一个内部子技能，由 `context7-auto-research` 主技能通过 Task 工具调用。

## 用途

独立执行 Context7 API 调用，使用 `context: fork` 避免携带主对话的上下文，减少 Token 消耗。

## 接收参数

通过 Task 工具的 prompt 参数接收完整命令：

1. **搜索库**：`node context7-api.js search <libraryName> <query>`
2. **获取文档**：`node context7-api.js context <libraryId> <query>`

## 执行命令示例

```bash
# 搜索库
node .claude/skills/context7-auto-research/context7-api.js search "react" "useEffect hook"

# 获取文档
node .claude/skills/context7-auto-research/context7-api.js context "/facebook/react" "useEffect cleanup"

# 带版本的文档查询
node .claude/skills/context7-auto-research/context7-api.js context "/vercel/next.js/v15.1.8" "middleware"
```

## 执行流程

1. **调用 API**：执行 context7-api.js 脚本
2. **返回 JSON**：直接返回 API 响应的 JSON 数据

## 输出格式

### 搜索库响应

```json
{
  "libraries": [
    {
      "id": "/facebook/react",
      "name": "React",
      "description": "A JavaScript library for building user interfaces",
      "trustScore": 98,
      "versions": ["v19.0.0", "v18.3.1"]
    }
  ]
}
```

### 获取文档响应

```json
{
  "results": [
    {
      "title": "useEffect",
      "content": "useEffect is a React Hook that lets you synchronize...",
      "source": "docs/reference/react/useEffect.md",
      "relevance": 0.95
    }
  ]
}
```

## 注意事项

- 脚本路径必须正确（相对于仓库根目录）
- API key 从 `.env` 文件或环境变量自动加载
- 网络错误会返回错误信息
- 超时时间默认由 Node.js 控制
