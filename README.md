# 快速开始指南

5 分钟配置 Context7 Auto Research Skill

## 1. 克隆仓库

```bash
git clone https://github.com/petaflops/context7-auto-research.git
cd context7-auto-research
```

## 2. 获取 API Key（可选但推荐）

访问 [context7.com/dashboard](https://context7.com/dashboard) 注册并获取免费 API key。

> 💡 不配置 API key 也可以使用，但会有较低的速率限制。

## 3. 配置 API Key

在 skill 目录下创建 `.env` 文件：

```bash
cd .claude/skills/auto-research
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
node .claude/skills/auto-research/context7-api.js search "react" "useEffect hook"

# 获取 Next.js 文档
node .claude/skills/auto-research/context7-api.js context "/vercel/next.js" "middleware"
```

如果看到 JSON 响应，说明配置成功！

## 5. 开始使用

Skill 会自动激活，无需手动调用。直接向 Claude 提问：

```
你：如何在 Next.js 15 中配置中间件？
```

Claude 会自动：
1. 检测到 "Next.js 15" 和 "配置中间件"
2. 调用 Context7 API 搜索 Next.js
3. 获取最新的中间件文档
4. 提供准确的答案和代码示例

## 常见问题

### Q: 我没有 API key 可以用吗？
A: 可以！不配置 API key 也能使用，只是有较低的速率限制。

### Q: .env 文件放在哪里？
A: 放在 `.claude/skills/auto-research/.env`

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

- 查看 [.claude/skills/auto-research/SKILL.md](./.claude/skills/auto-research/SKILL.md) 了解技术细节
- 开始提问，让 Claude 自动获取最新文档！

## 故障排除

### 脚本执行失败
```bash
# 确保脚本有执行权限
chmod +x .claude/skills/auto-research/context7-api.js

# 确保 Node.js 已安装
node --version  # 应该显示版本号
```

### API 返回 401 错误
检查 API key 是否正确配置：
```bash
# 查看 .env 文件
cat .claude/skills/auto-research/.env

# 确保格式正确
CONTEXT7_API_KEY=your_key_here  # ✅ 正确
CONTEXT7_API_KEY = your_key_here  # ❌ 错误（有空格）
```

### API 返回 429 错误
速率限制已达到，等待一段时间或升级 API key 配额。

---

🎉 配置完成！现在你可以享受自动文档研究功能了！
