# Context7 Auto Research Skill

[![Claude Code Skill](https://img.shields.io/badge/Claude%20Code-Skill-blue)](https://skillsmp.com/)
[![GitHub Stars](https://img.shields.io/github/stars/BenedictKing/context7-auto-research?style=social)](https://github.com/BenedictKing/context7-auto-research)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

[中文文档](./README_CN.md)

## Quick Start Guide

Set up Context7 Auto Research Skill in 5 minutes

## 1. Clone the Repository

```bash
git clone https://github.com/BenedictKing/context7-auto-research.git
cd context7-auto-research
```

## 2. Get API Key (Optional but Recommended)

Visit [context7.com/dashboard](https://context7.com/dashboard) to register and get a free API key.

> 💡 You can use this skill without an API key, but with lower rate limits.

## 3. Configure API Key

Create a `.env` file in the skill directory:

```bash
cd .claude/skills/context7-auto-research
cp .env.example .env
```

Edit the `.env` file and add your API key:

```bash
CONTEXT7_API_KEY=your_actual_api_key_here
```

## 4. Test the Script

Verify your configuration:

```bash
# Search for React library
node .claude/skills/context7-auto-research/context7-api.js search "react" "useEffect hook"

# Get Next.js documentation
node .claude/skills/context7-auto-research/context7-api.js context "/vercel/next.js" "middleware"
```

If you see JSON responses, your setup is successful!

## 5. Start Using

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
