# shelp - AI-Powered Shell Helper

Generate shell commands and scripts from natural language using Claude AI.

## Quick Start

### Bootstrap (Build shelp from scratch)

If you don't have `shelp` yet, build it from the prompt file:

```bash
cat shelp-PROMPT.md | claude --model claude-opus-4-5 -p
```

Or if you already have shelp, you can rebuild it:

```bash
./shelp --compile shelp-PROMPT.md
```

### Install

```bash
./shelp --install ~/bin
```

This will:
- Copy shelp to `~/bin`
- Optionally add `~/bin` to your PATH in `~/.bashrc`

## Usage

### One-Liner Mode (Quick Commands)

Just describe what you want:

```bash
shelp find all python files modified today
# Output: find . -name "*.py" -mtime -1

shelp count lines of code excluding node_modules
# Output: find . -name "*.js" -not -path "./node_modules/*" | xargs wc -l

shelp split input by commas and add @company.com to each
# Output: tr ',' '\n' | sed 's/$/@company.com/'
```

### Script Mode (Save to File)

Use `-o` to create a complete, executable script:

```bash
shelp -o backup.sh backup my home directory to /mnt/backup with rsync
shelp -o analyze.py parse nginx logs and show top 10 IPs by request count
shelp -o deploy.sh deploy docker containers with health checks and rollback
```

The script will be shown for confirmation before saving. You can edit the
prompt if the output isn't quite right.

### Compile Mode (From Prompt Files)

For complex tools, write detailed requirements in a file:

```bash
# Create mytool-PROMPT.md with your requirements
echo "Create a CLI tool that..." > mytool-PROMPT.md

# Compile it
shelp --compile mytool-PROMPT.md
```

This creates `mytool` from `mytool-PROMPT.md`. The prompt file is kept for
future improvements.

### Improve & Tweak

Iterate on your scripts:

```bash
# Edit the prompt and regenerate
shelp --improve backup.sh

# Quick tweak without full rewrite
shelp --tweak backup.sh

# Improve shelp itself!
shelp --improve
```

### View Prompts

See what prompt created a script:

```bash
shelp --prompt backup.sh
shelp --prompt              # View shelp's own prompt
```

## Options

| Option | Description |
|--------|-------------|
| `-o, --output FILE` | Save as executable script |
| `--compile FILE` | Compile from `-PROMPT.md` file |
| `--improve [FILE]` | Improve a script (default: shelp) |
| `--tweak [FILE]` | Quick tweaks to a script |
| `--prompt [FILE]` | Show prompt for file |
| `--install [DIR]` | Install to directory (default: ~/bin) |
| `--model MODEL` | Use specific Claude model |
| `--opus` | Use claude-opus-4-5 model |
| `--help` | Show help with examples |

## How It Works

1. **One-liner mode**: Generates concise shell commands optimized for
   copy-paste

2. **Script mode** (`-o`): Generates complete scripts with:
   - Proper shebang
   - Comments explaining the code
   - Error handling
   - The prompt embedded in the file for future improvements

3. **Compile mode** (`--compile`): Reads requirements from a `-PROMPT.md` file
   and creates the script. The prompt file is kept separate for easy editing.

4. **Improve/Tweak**: Opens your editor to modify the prompt, then regenerates
   the script with context from the original version for incremental updates.

## Requirements

- Python 3.6+
- [Claude CLI](https://github.com/anthropics/claude-code) installed and configured

## Examples

```bash
# Quick one-liners
shelp extract all URLs from a webpage
shelp find largest files in current directory
shelp convert JSON to CSV from stdin

# Scripts
shelp -o monitor.sh monitor disk usage and alert if over 90%
shelp -o scrape.py scrape product prices from a URL list

# Complex tools via compilation
cat > downloader-PROMPT.md << 'EOF'
Create a download manager that:
- Supports parallel downloads
- Shows progress bars
- Resumes interrupted downloads
- Validates checksums
EOF
shelp --compile downloader-PROMPT.md
```

## Tips

- Use `--opus` for complex tasks that benefit from more reasoning
- Start with one-liners, save good ones with `-o`
- Use `--tweak` for quick fixes, `--improve` for major changes
- The prompt is your documentation - keep it clear!

## Self-Improvement

shelp can improve itself:

```bash
shelp --improve
# Edit the prompt to add new features
# shelp regenerates with your changes
```

All improvements are tracked in a changelog embedded in the script.
