# shelp: AI-Powered Shell Script Generator

You are an expert programmer. Write a script called `shelp` (shell help) that uses AI to generate shell commands and scripts.

## Core Mechanism

The script uses the `claude` CLI tool to generate output:
```bash
claude -p "prompt"
```

For long prompts, pipe content to stdin instead of passing as arguments:
```bash
cat foo | claude -p
```

**Important:** The `claude` command outputs progress text to stderr. Only display stderr if claude returns a non-zero exit code.

---

## Basic Usage (One-Liner Mode)

When called with just a prompt: 
```bash
$ shelp split input by commas and output in lines with @meta.com added to each item
tr ',' '\n' | sed 's/^[ \t]*//; s/[ \t]*$//' | sed 's/$/@meta.com/'
```

Output a bash one-liner to stdout that accomplishes the task. The one-liner may use `python -c` if that's the cleanest solution. Just output a copyable shell command, no markdown.

---

## Output Modes

### File Output Mode (`-o <output>`)

```bash
shelp -o myscript.sh "process log files and extract errors"
```

Behavior:
1. Generate a full, well-commented executable script (not a one-liner)
2. Echo the script to stdout for user review
3. Prompt the user to confirm the script looks OK
4. If confirmed: write to file and make it executable
5. If rejected: open the prompt in `$EDITOR` for modification
   - If the user saves changes: regenerate with the modified prompt
   - If unchanged: abort

Language selection:
- `.sh` extension → bash
- `.py` extension → python
- No extension → AI chooses (prefer bash/python, perl one-liners OK if helpful)

**Prompt storage:** Embed the original prompt verbatim inside the generated script (as a comment). This metadata should NOT be shown during the confirmation step.

### Compile Mode (`--compile <command>-PROMPT.md`)

```bash
shelp --compile myscript-PROMPT.md
```

Reads the prompt from `<command>-PROMPT.md` and outputs to `<command>` (executable). The script metadata should point at the prompt MD file rather than embed the prompt itself.

---

## Improvement Modes

### `--improve <file>`

Opens `$EDITOR` with the prompt that created the file. After editing:
- The AI receives both the current script AND the updated prompt
- Generates an improved version based on the changes
- Maintains a changelog within the file

**Finding the prompt:**
- Use the metadata to find the prompt.
- For `--compile` scripts: read from the corresponding `PROMPT.md` file
- For `-o` scripts: extract from the embedded prompt in the script
- **Note:** Resolve symlinks to find the real path when looking for `PROMPT.md` files. they will sit in the real path of the executable.

### `--improve`

Improves `shelp` itself using its own prompt file.

### `--tweak [file]`

A variant of `--improve` for quick modifications:

1. Opens `$EDITOR` with the prompt commented out
2. Adds blank lines at the top (cursor starts here)
3. Includes instructions explaining the user only needs to describe desired changes
4. The AI first modifies the prompt based on the tweak, then regenerates the script

**For both improve and tweak:** Make incremental edits to the script rather than rewriting from scratch. In tweak mode, also incrementally update the prompt.

---

## Utility Commands

### `--prompt [file]`
Display the prompt that created the given file (including changelog).
If no file specified, display shelp's own prompt.

### `--help`
Show detailed usage instructions with examples.

### `--install [directory]`
Install shelp to the specified directory (default: `~/bin`).
- Create the directory if it doesn't exist
- Offer to add the directory to `PATH` in `.bashrc` if not already present

### `--model <model>`
Pass the specified model to claude via `--model`.

### `--opus`
Shorthand for `--model claude-opus-4-5`.

---

## AI Prompting Guidelines

When prompting the AI to generate scripts:
- Be expressive and clear about requirements
- Prefer bash and classic Unix tools
- Python is acceptable when appropriate
- Perl one-liners are OK if genuinely helpful

---

## Bootstrap Instructions

Since shelp doesn't exist yet, build it as if the user ran:
```bash
shelp --compile shelp-PROMPT.md
```

### Output Requirements

1. **Generate `shelp`** - the executable script

2. **Generate `README.md`** explaining:
   - How to bootstrap (run `cat shelp-PROMPT.md | claude --model claude-opus-4-5 -p`)
   - Complete usage instructions
   - Update this README for major changes made via `--tweak` or `--improve`

3. **Display a terminal-friendly getting-started guide** (max 100 chars/line, no markdown, emoji OK):
   - Start with basic examples (stdout one-liners)
   - Progress to file output with `-o`
   - Cover compilation mode
   - Make examples engaging and realistic
   - Encourage using `--install`


---

## Changelog

[2025-12-03 13:38] Tweaked: claude doesn't always output the entire script as ...
