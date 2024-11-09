
# <p align="center"><img src="https://github.com/user-attachments/assets/896f9239-134a-4428-9bb5-50ea59cdb5c3" alt="lumen" /></p>
[![Crates.io Total Downloads](https://img.shields.io/crates/d/lumen?label=downloads%20%40crates.io)](https://crates.io/crates/lumen)
![GitHub License](https://img.shields.io/github/license/jnsahaj/lumen)
![Crates.io Size](https://img.shields.io/crates/size/lumen)




lumen is a command-line tool that uses AI to generate commit messages, summarise git diffs or past commits, and more without requiring an API key.

![demo](https://github.com/user-attachments/assets/0d029bdb-3b11-4b5c-bed6-f5a91d8529f2)

# Features
- Generate commit message for staged changes
- Generate summary for changes in a git commit by providing its [SHA-1](https://graphite.dev/guides/git-hash)
- Generate summary for changes in git diff (staged/unstaged)
- Fuzzy-search for the commit to generate a summary
- Free and unlimited - no API key required to work out of the box
- Pretty output formatting enabled by Markdown
- Supports multiple AI providers

# Usage
Try `lumen --help`

To generate a commit message for staged changes
```zsh
lumen draft
```
The commit message can be piped to other commands
```zsh
# copy the commit message to clipboard (macos and linux, respectively)
lumen draft | pbcopy
lumen draft | xclip -selection clipboard

# open the commit message in your code editor
lumen draft | code -

# directly commit with the generated message
lumen draft | git commit -F -
```
The AI generates more meaningful commit messages when you provide context.

```zsh
lumen draft
# Output: "feat(button.tsx): Change button color to blue"

lumen draft --context "match brand guidelines"
# Output: "feat(button.tsx): Update button color to align with brand identity"
```
To summarise a commit, pass in its SHA-1 
```zsh
lumen explain HEAD
lumen explain cc50651f
```
To use the interactive fuzzy-finder (requires: fzf)
```zsh
lumen list
```
To generate a summary for the current git diff
```zsh
lumen explain --diff
lumen explain --diff --staged
```
You can ask a question about the diff (or a commit) using `--query`
```zsh
lumen explain --diff --query "how will this change affect performance?"
lumen explain HEAD~2 --query "how can this be improved?"
```

AI Provider can be configured by using CLI arguments or Environment variables.
```sh
-p, --provider <PROVIDER>  [env: LUMEN_AI_PROVIDER] [default: phind] [possible values: openai, phind, groq, claude, ollama]
-k, --api-key <API_KEY>    [env: LUMEN_API_KEY]
-m, --model <MODEL>        [env: LUMEN_AI_MODEL]

# eg: lumen -p="openai" -k="<your-api-key>" -m="gpt-4o" explain HEAD
# eg: lumen -p="openai" -k="<your-api-key>" -m="gpt-4o" draft
# eg: LUMEN_AI_PROVIDER="openai" LUMEN_API_KEY="<your-api-key>" LUMEN_AI_MODEL="gpt-4o" lumen list
```

### Supported providers

| Provider                                                                                                             | API Key Required | Models                                                                                      |
|----------------------------------------------------------------------------------------------------------------------|------------------|---------------------------------------------------------------------------------------------|
| [Phind](https://www.phind.com/agent) `phind` (Default)                                                             | No              | `Phind-70B`                                                                                |
| [Groq](https://groq.com/) `groq`                                                                                   | Yes (free)      | `llama2-70b-4096`, `mixtral-8x7b-32768` (default: `mixtral-8x7b-32768`)                     |
| [OpenAI](https://platform.openai.com/docs/guides/text-generation/chat-completions-api) `openai`                    | Yes             | `gpt-4o`, `gpt-4o-mini`, `gpt-4`, `gpt-3.5-turbo` (default: `gpt-4o-mini`)                  |
| [Claude](https://claude.ai/new) `claude`                                                                     | Yes             | [see list](https://docs.anthropic.com/en/docs/about-claude/models#model-names) (default: `claude-3-5-sonnet-20241022`) |                                                                                |
| [Ollama](https://github.com/ollama/ollama) `ollama`                                                                     | No (local)             | [see list](https://github.com/ollama/ollama/blob/main/docs/api.md#model-names) (required) |                                                                                |


# Installation
### Using [Homebrew](https://brew.sh/) (MacOS and Linux)
```
brew tap jnsahaj/lumen
brew install --formula lumen
```
### Using [Cargo](https://github.com/rust-lang/cargo)

> [!IMPORTANT]
> `cargo` is a package manager for `rust`,
> and is installed automatically when you install `rust`.
> see [installation guide](https://doc.rust-lang.org/cargo/getting-started/installation.html)
```
cargo install lumen
```

# Prerequisites
1. git
2. [fzf](https://github.com/junegunn/fzf) (optional): Required for `lumen list` command
3. [mdcat](https://github.com/swsnr/mdcat) (optional): Required for pretty output formatting
