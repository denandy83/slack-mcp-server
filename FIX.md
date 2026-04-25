# 🛠 How to Fix "Missing Numbers" (Message Scrubbing)

If your Slack messages are missing numbers (e.g., "15 mins late" becomes "mins late"), it's because you are running a **stale binary** from the NPM package. 

Even if you fork this repository, the default setup uses pre-compiled files that contain an old "stopword" filter. Follow these steps to fix it on your machine:

### 1. Compile the Source Code
The Go source code in this repo is already fixed. You just need to turn it into an executable. Navigate to the root of your fork and run:

```bash
go build -o slack-mcp-server-fixed cmd/slack-mcp-server/main.go
```

### 2. Update your Configuration
You must tell your AI client (Gemini or Claude) to use the **new binary you just built**, instead of the NPM version.

1. Get the full path to your new binary:
   ```bash
   echo $(pwd)/slack-mcp-server-fixed
   ```
2. Open your `settings.json` (Gemini) or `claude_desktop_config.json` (Claude).
3. Update the `slack-stealth` (or `slack`) entry:

**❌ OLD (Broken):**
```json
"slack-stealth": {
  "command": "node",
  "args": ["/path/to/bin/index.js"],
  "env": { ... }
}
```

**✅ NEW (Fixed):**
```json
"slack-stealth": {
  "command": "/THE/FULL/PATH/YOU/GOT/IN/STEP/2",
  "args": [],
  "env": { ... }
}
```

### 3. Restart
Restart your Gemini/Claude session. Numbers and punctuation will now display correctly.
