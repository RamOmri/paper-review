# paper-review

`paper-review` asks three AI models to peer-review a research paper, then combines their reviews into one report.

```
paper-review path/to/paper.pdf
```

The command writes `path/to/paper_review.pdf` next to the input. The PDF starts with the combined report, followed by the three original reviews.

## How it works

1. Gemini, GPT and Claude review the paper in parallel, and all three get the instructions in `review_prompt.md`. Those instructions ask for a review against general academic standards, covering methodology, results, novelty, related work and writing.
2. GPT then follows `aggregate_prompt.md` to combine the reviews. It checks every point against the paper and marks it as confirmed, partly right or wrong. It groups the points by how many reviewers raised them and ends with a list of changes, most important first.

A run takes about 30 to 60 minutes. Each run keeps its working files and logs in a `runs/` folder next to the script.

## Requirements

- macOS or Linux.
- The [Antigravity CLI](https://antigravity.google/product/antigravity-cli) (`agy`), signed in with a Google account. On macOS, install it with `brew install --cask antigravity-cli`.
- The [Codex CLI](https://github.com/openai/codex) (`codex`), installed with `npm install -g @openai/codex` and signed in with `codex login`. The copy bundled with the VS Code ChatGPT extension also works.
- [Claude Code](https://code.claude.com/docs/en/setup) (`claude`), signed in.
- poppler and pandoc. On macOS, install them with `brew install poppler pandoc`.
- Google Chrome or Chromium, which turns the report into a PDF.

## Install

```
git clone https://github.com/RamOmri/paper-review.git
mkdir -p ~/.local/bin
ln -s "$PWD/paper-review/paper-review" ~/.local/bin/paper-review
```

Check that `~/.local/bin` is on your `PATH`.

## Models

The models are set at the top of the `paper-review` script.

| Reviewer | Default model | Effort |
|---|---|---|
| Gemini, through Antigravity | `gemini-3.8-flash-high` | High |
| Claude, through Claude Code | `claude-opus-5-5` | max |
| GPT, through Codex, which also combines the reviews | `gpt-6-astra` | ultra, or max if ultra is refused |

If your subscription does not include one of these models, change its name in the script. A reviewer that fails is skipped, and the report says so. The combining step needs Codex.

## Privacy and cost

The tool sends your paper to Google, OpenAI and Anthropic, so only use it on work you can share with them. Each run uses your own subscriptions, and ultra effort can use a large share of your Codex limits.

## License

MIT
