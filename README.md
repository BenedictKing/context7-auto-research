# Context7 Auto Research Skill

[![Claude Code Skill](https://img.shields.io/badge/Claude%20Code-Skill-blue)](https://skillsmp.com/)
[![GitHub Stars](https://img.shields.io/github/stars/BenedictKing/context7-auto-research?style=social)](https://github.com/BenedictKing/context7-auto-research)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

English | [简体中文](./README_CN.md)

> 🚀 Automatically fetch the latest library and framework documentation for Claude Code, say goodbye to outdated training data!

## Introduction

Context7 Auto Research is an intelligent Claude Code skill that automatically fetches the latest documentation from Context7 when you ask questions about libraries, frameworks, or APIs. No manual invocation needed - it's fully automated!

### Key Features

- ✨ **Auto-Trigger**: Automatically activates when library/framework questions are detected
- 🎯 **Smart Matching**: Automatically selects the best documentation source based on trust score and version
- 🔄 **Real-Time Docs**: Fetches the latest documentation via Context7 (synced from GitHub and other sources)
- 🌐 **Wide Support**: Supports thousands of open-source libraries including React, Next.js, Prisma, Tailwind, and more
- 🏗️ **Efficient Architecture**: Uses dual-skill architecture to reduce token consumption
- 🌍 **Bilingual Support**: Supports both English and Chinese trigger keywords

## Quick Start

Set up in 5 minutes

## Installation

### Option 1: Install via add-skill (Recommended)

The easiest way to install this skill is using the `add-skill` tool:

```bash
# Install to Claude Code
npx add-skill BenedictKing/context7-auto-research

# Or install globally to all detected agents
npx add-skill BenedictKing/context7-auto-research -g
```

The skill will be automatically installed to `~/.claude/skills/context7-auto-research` and loaded by Claude Code.

### Option 2: Manual Installation via Git Clone

If you prefer manual installation or want to customize the setup:

#### 1. Clone the Repository

```bash
# Clone to Claude Code's skills directory
git clone https://github.com/BenedictKing/context7-auto-research.git ~/.claude/skills/context7-auto-research

# Or clone to your preferred location
git clone https://github.com/BenedictKing/context7-auto-research.git
cd context7-auto-research
```

#### 2. Get API Key (Optional but Recommended)

Visit [context7.com/dashboard](https://context7.com/dashboard) to register and get a free API key.

> 💡 You can use this skill without an API key, but with lower rate limits.

#### 3. Configure API Key

Create a `.env` file in the skill directory:

```bash
cd .claude/skills/context7-auto-research
cp .env.example .env
```

Edit the `.env` file and add your API key:

```bash
CONTEXT7_API_KEY=your_actual_api_key_here
```

#### 4. Test the Script

Verify your configuration:

```bash
# Search for React library
node .claude/skills/context7-auto-research/context7-api.js search "react" "useEffect hook"

# Get Next.js documentation
node .claude/skills/context7-auto-research/context7-api.js context "/vercel/next.js" "middleware"
```

If you see JSON responses, your setup is successful!

## Usage

The skill activates automatically - no manual invocation needed. Just ask Claude:

```
You: How do I configure middleware in Next.js 15?
```

Claude will automatically:
1. Detect "Next.js 15" and "configure middleware"
2. Use Task tool to call context7-fetcher sub-skill to search for Next.js
3. Select the best matching version (v15.x)
4. Use Task tool to call context7-fetcher to fetch middleware documentation
5. Integrate documentation and provide accurate answers with code examples

**Architecture Benefits:**
- Main skill understands your intent and context
- Sub-skill executes API calls independently (using `context: fork`)
- Reduces token consumption and improves response speed

## How It Works

### Auto-Trigger Mechanism

The skill automatically activates when detecting these keywords:

**Implementation Queries**
- Chinese: 如何实现、怎么写、怎么做
- English: How do I, How to, Show me how to

**Configuration & Setup**
- Chinese: 配置、设置、安装、初始化
- English: configure, setup, install, initialize

**Documentation Requests**
- Chinese: 文档、参考、API、查看
- English: documentation, docs, reference, look up

**Library/Framework Mentions**
- Frontend: React, Vue, Angular, Svelte, Solid
- Full-stack: Next.js, Nuxt, Remix, Astro
- Backend: Express, Fastify, Koa, Hono
- ORM: Prisma, Drizzle, TypeORM
- Services: Supabase, Firebase, Clerk
- UI: Tailwind, shadcn/ui, Radix
- Plus any npm package or GitHub repository

### Dual-Skill Architecture

This project uses a **two-stage architecture**, inspired by the `codex-review` design pattern:

```
User Query → Main Skill (context7-auto-research)
              ↓ Detect triggers + Analyze intent
         Task Tool → Sub-Skill (context7-fetcher)
              ↓ Search library (independent context)
         Main Skill ← Return search results
              ↓ Select best match
         Task Tool → Sub-Skill (context7-fetcher)
              ↓ Fetch docs (independent context)
         Main Skill ← Return documentation
              ↓ Integrate and generate response
         User ← Accurate answer + Code examples
```

**Why this design?**

| Aspect | Main Skill | Sub-Skill |
|--------|-----------|-----------|
| Context | Full conversation | Fork (independent) |
| Purpose | Intent analysis | API execution |
| Token usage | Higher | Lower |
| Execution | Sequential | Can be parallel |

**Benefits:**
- Main skill needs to understand user intent (requires context)
- API calls don't need conversation history (avoids wasting tokens)
- Separation improves efficiency and reduces costs

## FAQ

### Q: Can I use this without an API key?
A: Yes! The skill works without an API key, just with lower rate limits.

### Q: Where should I put the .env file?
A: Place it at `.claude/skills/context7-auto-research/.env`

### Q: How do I know if the skill is working?
A: When you ask questions about libraries/frameworks, Claude will automatically call the script to fetch documentation. You'll see up-to-date, accurate information in the responses.

### Q: Which libraries are supported?
A: All open-source libraries with documentation on GitHub, including:
- React, Vue, Angular, Svelte
- Next.js, Nuxt, Remix
- Prisma, Drizzle, TypeORM
- Express, Fastify, Koa
- Supabase, Firebase
- Tailwind, shadcn/ui
- And many more...

### Q: How do I specify a particular version?
A: Mention the version number in your question, for example:
```
How do I use the use hook in React 19?
Show me Next.js 15 middleware examples
```

## Example Conversations

### Example 1: React Hooks
```
You: What's new in React 19's useEffect?

Claude: [Automatically calls Context7 API]
According to the latest React 19 documentation...
[Provides accurate React 19 information]
```

### Example 2: Next.js Configuration
```
You: How do I set up environment variables in Next.js 15?

Claude: [Automatically calls Context7 API]
In Next.js 15, environment variables are configured by...
[Provides latest configuration methods and code examples]
```

### Example 3: Prisma Schema
```
You: Show me how to define a many-to-many relation in Prisma

Claude: [Automatically calls Context7 API]
Here's how to define many-to-many relations in Prisma...
[Provides Prisma schema examples]
```

## Next Steps

- Check [.claude/skills/context7-auto-research/SKILL.md](./.claude/skills/context7-auto-research/SKILL.md) for technical details
- Check [.claude/skills/context7-fetcher.md](./.claude/skills/context7-fetcher.md) for sub-skill architecture
- Start asking questions and let Claude automatically fetch the latest documentation!

## Architecture Overview

This project uses a **two-stage architecture**, inspired by the `codex-review` design pattern:

### Main Skill (context7-auto-research)
- Requires conversation context
- Detects trigger words and user intent
- Selects best matching library and version
- Integrates documentation into responses

### Sub-Skill (context7-fetcher)
- Runs independently using `context: fork`
- Purely executes API calls
- Doesn't carry conversation history
- Reduces token consumption

**Why this design?**
- Main skill needs to understand user intent (requires context)
- API calls don't need conversation history (wastes tokens)
- Separation improves efficiency and reduces costs

## Troubleshooting

### Script Execution Fails
```bash
# Ensure script has execute permissions
chmod +x .claude/skills/context7-auto-research/context7-api.js

# Ensure Node.js is installed
node --version  # Should display version number
```

### API Returns 401 Error
Check if API key is correctly configured:
```bash
# View .env file
cat .claude/skills/context7-auto-research/.env

# Ensure correct format
CONTEXT7_API_KEY=your_key_here  # ✅ Correct
CONTEXT7_API_KEY = your_key_here  # ❌ Wrong (has spaces)
```

### API Returns 429 Error
Rate limit reached. Wait for some time or upgrade your API key quota.

---

🎉 Setup complete! Now you can enjoy automatic documentation research!
