# vibecode-cli

You have access to `vibecode-cli`, a command line tool for building and deploying applications on [vibecode.dev](https://vibecode.dev) cloud infrastructure. Follow these steps exactly.

## Step 1: Check for the API key

```bash
echo "$VIBECODE_API_KEY"
```

If empty or unset, ask the user to provide their Vibecode API key. Do not continue without it.
The user can find their API key at vibecode.dev/key.

Once you have the key, export it and persist it so it survives across sessions:

```bash
export VIBECODE_API_KEY="the-key"
# Persist to shell profile
echo 'export VIBECODE_API_KEY="the-key"' >> ~/.bashrc
# Also source it for the current session if needed
```

Use `~/.zshrc` instead if the user's shell is zsh. If you cannot determine the shell, write to both `~/.bashrc` and `~/.zshrc`.

## Step 2: Install vibecode-cli

Check if already installed:

```bash
vibecode-cli user
```

If that succeeds, skip to Step 3.

If the command is not found, detect the platform and install from the correct URL:

| Platform | URL |
|---|---|
| macOS Apple Silicon | `https://github.com/vibecode/vibecode-cli/releases/download/v0.2.0/vibecode-cli-darwin-arm64` |
| macOS Intel | `https://github.com/vibecode/vibecode-cli/releases/download/v0.2.0/vibecode-cli-darwin-amd64` |
| Linux x86_64 | `https://github.com/vibecode/vibecode-cli/releases/download/v0.2.0/vibecode-cli-linux-amd64` |
| Linux arm64 | `https://github.com/vibecode/vibecode-cli/releases/download/v0.2.0/vibecode-cli-linux-arm64` |
| Windows x86_64 | `https://github.com/vibecode/vibecode-cli/releases/download/v0.2.0/vibecode-cli-windows-amd64` |

**Linux / macOS (try in order):**

```bash
# Option A: system-wide (requires write access to /usr/local/bin)
curl --fail --silent --show-error --location <URL> --output /usr/local/bin/vibecode-cli && chmod +x /usr/local/bin/vibecode-cli

# Option B: user-local
mkdir -p ~/.local/bin
curl --fail --silent --show-error --location <URL> --output ~/.local/bin/vibecode-cli && chmod +x ~/.local/bin/vibecode-cli
# Ensure ~/.local/bin is in PATH

# Option C: current directory fallback
curl --fail --silent --show-error --location <URL> --output ./vibecode-cli && chmod +x ./vibecode-cli
# Invoke as ./vibecode-cli
```

**Windows:**

```powershell
Invoke-WebRequest -Uri <URL> -OutFile "$env:LOCALAPPDATA\\vibecode-cli.exe"
```

After installing, verify with `vibecode-cli user`.

## Step 3: Create the skill reference

Run `vibecode-cli skill` and save the output as a skill file so it is always available in your context. This file contains every command, flag, and workflow you need.

**Claude Code:**

```bash
mkdir -p ~/.claude/skills/vibecode-cli
vibecode-cli skill >~/.claude/skills/vibecode-cli/SKILL.md
```

**Codex:**

```bash
mkdir -p codex/skills
vibecode-cli skill >codex/skills/SKILL.md
```

Then add `codex/skills/SKILL.md` to your Codex instructions or context.

**Gemini CLI:**

```bash
mkdir -p ~/.gemini/skills
vibecode-cli skill >~/.gemini/skills/vibecode-cli.md
```

**OpenClaw:**

```bash
mkdir -p ~/.openclaw/skills
vibecode-cli skill >~/.openclaw/skills/vibecode-cli.md
```

**Other agents:**

The goal is to create a persistent skill file that your agent loads into context automatically. Run `vibecode-cli skill`, save the output to a file in whatever location your agent reads from, and ensure it gets loaded at the start of each session.

Once the skill file is created, read it and follow the instructions inside it.

## Step 4: Ask the user what they want to build

If the user hasn't already told you, ask whether they want to create a **web app** or **mobile app**. Then follow the workflows described in the skill reference.

If you know information about the user, lookup the recent tasks or memories the user has, and give the user suggestions of web apps they could make!
