---
title: "Fix Ollama Version Mismatch Warning: ollama version is ... Warning: client version is ..."
date: 2025-08-04
tags: ["ai", "dev", "tutorial"]
description: "Resolve 'client version is x.y.z' warnings when Ollama brew service runs an outdated server version"
---

## The Problem

If you see this warning when running `ollama --version`, smth like:

```
ollama version is 0.9.2
Warning: client version is 0.10.1
```

Your Ollama client and server are running different versions.

## Root Cause

Brew upgraded the Ollama binary but the background service is still running the old server version. The `brew services` daemon continues using the cached older version.

## Solution (for Mac)

### Step 1: Stop All Ollama Processes

```bash
# stop brew service
brew services stop ollama

# kill any remaining processes
sudo kill $(pgrep ollama)

# verify nothing is running
pgrep ollama
```

### Step 2: Reinstall and Restart

```bash
# reinstall to ensure clean state
brew reinstall ollama

# start service with new version
brew services start ollama

# verify versions match
ollama --version
```

## Why This Works

- `brew services stop` stops the daemon but doesn't always kill the process
- `pgrep ollama` finds process IDs for any running Ollama instances
- `brew reinstall` ensures the service uses the latest binary
- Fresh service start loads the correct version

This approach completely resets Ollama's state and eliminates version conflicts between client and server.