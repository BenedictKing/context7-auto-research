# Context7 Auto Research Skill

[![Claude Code Skill](https://img.shields.io/badge/Claude%20Code-Skill-blue)](https://skillsmp.com/)
[![GitHub Stars](https://img.shields.io/github/stars/BenedictKing/context7-auto-research?style=social)](https://github.com/BenedictKing/context7-auto-research)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

[English](./README.md) | 简体中文

> 🚀 让 Claude Code 自动获取最新的库和框架文档，告别过时的训练数据！

## 简介

Context7 Auto Research 是一个智能的 Claude Code 技能，当你询问关于库、框架或 API 的问题时，它会自动从 Context7 获取最新的文档。无需手动调用，完全自动化！

### 核心特性

- ✨ **自动触发**：检测到库/框架相关问题时自动激活
- 🎯 **智能匹配**：根据信任分数和版本自动选择最佳文档源
- 🔄 **实时文档**：通过 Context7 获取最新文档（从 GitHub 等上游同步）
- 🌐 **广泛支持**：支持 React、Next.js、Prisma、Tailwind 等数千个开源库
- 🏗️ **高效架构**：采用双技能架构，减少 Token 消耗
- 🌍 **双语支持**：支持中英文触发词

## 快速开始

5 分钟完成配置

## 安装方式

### 方式一：使用 add-skill 安装（推荐）

最简单的安装方式是使用 `add-skill` 工具：

```bash
# 安装到 Claude Code
npx add-skill BenedictKing/context7-auto-research

# 或全局安装到所有检测到的 AI 编程助手（Claude Code、Cursor、Codex 等）
npx add-skill BenedictKing/context7-auto-research -g
```

Skill 会自动安装到 `~/.claude/skills/context7-auto-research` 并被 Claude Code 加载。

### 方式二：通过 Git Clone 手动安装

如果你偏好手动安装或想自定义设置：

#### 1. 克隆仓库

```bash
# 克隆到 Claude Code 的 skills 目录
git clone https://github.com/BenedictKing/context7-auto-research.git ~/.claude/skills/context7-auto-research

# 或克隆到你偏好的位置
git clone https://github.com/BenedictKing/context7-auto-research.git
cd context7-auto-research
```

## 2. 获取 API Key（可选但推荐）

访问 [context7.com/dashboard](https://context7.com/dashboard) 注册并获取免费 API key。

> 💡 不配置 API key 也可以使用，但会有较低的速率限制。

## 3. 配置 API Key

在 skill 目录下创建 `.env` 文件：

```bash
cd .claude/skills/context7-auto-research
cp .env.example .env
```

编辑 `.env` 文件，填入你的 API key：

```bash
CONTEXT7_API_KEY=your_actual_api_key_here
```

## 4. 测试脚本

验证配置是否正确：

```bash
# 搜索 React 库
node .claude/skills/context7-auto-research/context7-api.js search "react" "useEffect hook"

# 获取 Next.js 文档
node .claude/skills/context7-auto-research/context7-api.js context "/vercel/next.js" "middleware"
```

如果看到 JSON 响应，说明配置成功！

## 5. 开始使用

Skill 会自动激活，无需手动调用。直接向 Claude 提问：

```
你：如何在 Next.js 15 中配置中间件？
```

Claude 会自动：
1. 检测到 "Next.js 15" 和 "配置中间件"
2. 使用 Task 工具调用 context7-fetcher 子技能搜索 Next.js
3. 选择最佳匹配的版本（v15.x）
4. 使用 Task 工具调用 context7-fetcher 获取中间件文档
5. 整合文档内容，提供准确的答案和代码示例

**架构优势：**
- 主技能理解你的意图和上下文
- 子技能独立执行 API 调用（使用 `context: fork`）
- 减少 Token 消耗，提高响应速度

## 工作原理

### 自动触发机制

技能会在检测到以下关键词时自动激活：

**实现相关**
- 中文：如何实现、怎么写、怎么做
- 英文：How do I、How to、Show me how to

**配置相关**
- 中文：配置、设置、安装、初始化
- 英文：configure、setup、install、initialize

**文档相关**
- 中文：文档、参考、API、查看
- 英文：documentation、docs、reference、look up

**库/框架提及**
- 前端框架：React、Vue、Angular、Svelte、Solid
- 全栈框架：Next.js、Nuxt、Remix、Astro
- 后端框架：Express、Fastify、Koa、Hono
- ORM：Prisma、Drizzle、TypeORM
- 服务：Supabase、Firebase、Clerk
- UI 库：Tailwind、shadcn/ui、Radix
- 以及任何 npm 包或 GitHub 仓库

### 双技能架构

本项目采用**两阶段架构**：

```
用户提问 → 主技能 (context7-auto-research)
              ↓ 检测触发词 + 分析意图
         Task 工具 → 子技能 (context7-fetcher)
              ↓ 搜索库（独立上下文）
         主技能 ← 返回搜索结果
              ↓ 选择最佳匹配
         Task 工具 → 子技能 (context7-fetcher)
              ↓ 获取文档（独立上下文）
         主技能 ← 返回文档内容
              ↓ 整合并生成回答
         用户 ← 准确的答案 + 代码示例
```

**为什么这样设计？**

| 方面 | 主技能 | 子技能 |
|------|--------|--------|
| 上下文 | 完整对话历史 | Fork（独立） |
| 用途 | 意图分析 | API 执行 |
| Token 消耗 | 较高 | 较低 |
| 执行方式 | 顺序 | 可并行 |

**优势：**
- 主技能需要理解用户意图（需要上下文）
- API 调用不需要对话历史（避免浪费 Token）
- 分离后提高效率，降低成本

## 常见问题

### Q: 我没有 API key 可以用吗？
A: 可以！不配置 API key 也能使用，只是有较低的速率限制。

### Q: .env 文件放在哪里？
A: 放在 `.claude/skills/context7-auto-research/.env`

### Q: 如何知道 skill 是否在工作？
A: 当你询问库/框架相关问题时，Claude 会自动调用脚本获取文档。你可以在响应中看到最新的、准确的信息。

### Q: 支持哪些库？
A: 支持所有在 GitHub 上有文档的开源库，包括：
- React, Vue, Angular, Svelte
- Next.js, Nuxt, Remix
- Prisma, Drizzle, TypeORM
- Express, Fastify, Koa
- Supabase, Firebase
- Tailwind, shadcn/ui
- 以及更多...

### Q: 如何指定特定版本？
A: 在问题中提到版本号，例如：
```
如何在 React 19 中使用 use hook？
Show me Next.js 15 middleware examples
```

## 示例对话

### 示例 1：React Hooks
```
你：React 19 的 useEffect 有什么变化？

Claude：[自动调用 Context7 API]
根据 React 19 的最新文档...
[提供准确的 React 19 信息]
```

### 示例 2：Next.js 配置
```
你：怎么在 Next.js 15 中设置环境变量？

Claude：[自动调用 Context7 API]
在 Next.js 15 中，环境变量的配置方式是...
[提供最新的配置方法和代码示例]
```

### 示例 3：Prisma Schema
```
你：Show me how to define a many-to-many relation in Prisma

Claude：[自动调用 Context7 API]
Here's how to define many-to-many relations in Prisma...
[提供 Prisma schema 示例]
```

## 下一步

- 查看 [.claude/skills/context7-auto-research/SKILL.md](./.claude/skills/context7-auto-research/SKILL.md) 了解技术细节
- 查看 [.claude/skills/context7-fetcher.md](./.claude/skills/context7-fetcher.md) 了解子技能架构
- 开始提问，让 Claude 自动获取最新文档！

## 架构说明

本项目采用**两阶段架构**：

### 主技能 (context7-auto-research)
- 需要对话上下文
- 检测触发词和用户意图
- 选择最佳匹配的库和版本
- 整合文档到响应中

### 子技能 (context7-fetcher)
- 使用 `context: fork` 独立运行
- 纯粹执行 API 调用
- 不携带对话历史
- 减少 Token 消耗

**为什么这样设计？**
- 主技能需要理解用户意图（需要上下文）
- API 调用不需要对话历史（浪费 Token）
- 分离后提高效率，降低成本

## 故障排除

### 脚本执行失败
```bash
# 确保脚本有执行权限
chmod +x .claude/skills/context7-auto-research/context7-api.js

# 确保 Node.js 已安装
node --version  # 应该显示版本号
```

### API 返回 401 错误
检查 API key 是否正确配置：
```bash
# 查看 .env 文件
cat .claude/skills/context7-auto-research/.env

# 确保格式正确
CONTEXT7_API_KEY=your_key_here  # ✅ 正确
CONTEXT7_API_KEY = your_key_here  # ❌ 错误（有空格）
```

### API 返回 429 错误
速率限制已达到，等待一段时间或升级 API key 配额。

---

🎉 配置完成！现在你可以享受自动文档研究功能了！
