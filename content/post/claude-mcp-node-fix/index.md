---
title: "Fix Claude MCP Playwright Error: Node.js Version Conflict with Bun"
date: 2025-07-02
tags: ["ai", "dev", "tool"]
description: "Solve 'performance is not defined' and Node.js version conflicts when setting up Playwright MCP server in Claude Desktop with Bun runtime"
---

## The Problem

When setting up Playwright MCP server in Claude Desktop on macOS with Bun, you might encounter these errors:

```
ReferenceError: performance is not defined
ERROR: npm v10.9.0 is known not to run on Node.js v14.19.2
SyntaxError: Unexpected token '&&='
```

This happens because Playwright requires Node.js APIs that aren't available in Bun's runtime environment.

## Root Cause

The issue occurs when:
1. Using `bunx` to run Node.js-based MCP servers like Playwright
2. npx finds the wrong Node.js version in PATH (e.g., v14 instead of v22)
3. Modern npm/Node.js features conflict with older Node.js versions

## Solution

Instead of using `bunx`, configure Claude to use Node.js directly with explicit PATH control:

### Step 1: Update Claude MCP Configuration

Edit your `claude_desktop_config.json`:

```json
{
  "mcpServers": {
    "playwright": {
      "command": "/Users/username/.nvm/versions/node/v22.11.0/bin/npx",
      "args": ["@playwright/mcp@latest"],
      "env": {
        "PATH": "/Users/username/.nvm/versions/node/v22.11.0/bin:/opt/homebrew/bin:/usr/local/bin:/usr/bin:/bin"
      }
    },
    "brave-search": {
      "command": "/Users/username/.nvm/versions/node/v22.11.0/bin/npx", 
      "args": ["-y", "@modelcontextprotocol/server-brave-search"],
      "env": {
        "BRAVE_API_KEY": "your-api-key",
        "PATH": "/Users/username/.nvm/versions/node/v22.11.0/bin:/opt/homebrew/bin:/usr/local/bin:/usr/bin:/bin"
      }
    }
  }
}
```

### Step 2: Key Changes Explained

- **Direct Node.js path**: Use full path to Node.js v22 npx
- **Explicit PATH**: Ensure Node.js v22 comes first in PATH environment
- **Remove bunx**: Don't use Bun for Node.js-specific packages

### Step 3: Verify Node.js Version

Check your Node.js installation:

```bash
/Users/username/.nvm/versions/node/v22.11.0/bin/node --version
# Should output: v22.11.0
```

## Why This Works

1. **Compatibility**: Node.js packages run in their native environment
2. **PATH priority**: Node.js v22 takes precedence over older versions
3. **Environment isolation**: Each MCP server gets its own environment

## Alternative: Use Extensions

For simpler setup, consider using Claude Desktop extensions instead of manual MCP configuration. Extensions handle Node.js version management automatically.

## Conclusion

When mixing Bun and Node.js in Claude MCP servers, use the right tool for each job. Keep Bun for Bun-compatible packages and use Node.js directly for Node.js-specific packages like Playwright.

This approach ensures reliable MCP server functionality without runtime conflicts.