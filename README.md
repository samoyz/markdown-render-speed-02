# markdown-render-speed-02

[![make.fun · markdown-render-speed-02](https://make.fun/badge/markdown-render-speed-02.svg)](https://make.fun/bounty/markdown-render-speed-02?utm_source=github&utm_medium=readme&utm_campaign=markdown-render-speed-02)
[![rank](https://make.fun/badge/markdown-render-speed-02/samoyz/markdown-render-speed-02/rank.svg)](https://make.fun/bounty/markdown-render-speed-02/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=markdown-render-speed-02)
[![reward](https://make.fun/badge/markdown-render-speed-02/samoyz/markdown-render-speed-02/reward.svg)](https://make.fun/bounty/markdown-render-speed-02?utm_source=github&utm_medium=readme&utm_campaign=markdown-render-speed-02)

A faster Markdown renderer: a fork of [marked](https://github.com/markedjs/marked) 18.1.3 (MIT, see `vendor/marked/`), profiled on the practice set and optimized for speed while returning byte-identical HTML for every input — `parse(markdown)` matches `marked.parse(markdown)` with default options (GFM on, async off) on all 720 practice cases. Entry for make.fun's "Make Markdown rendering faster" bounty.

## How it is faster

- First-character dispatch in the inline tokenizer instead of trying every inline regex per token.
- A combined-regex fast path for inline text runs, with a full-regex fallback for rare stops (protocols, `www.`, emails, hard breaks).
- Single-scan replacements for the table cell splitter, the trailing-blank-line trim, and the double-blank-line check.
- First-char and second-line guards that skip the table, `emStrong`/`del` delimiter-scan, and entity/percent work when the trigger characters are absent.
- Code-point helpers instead of spread allocations in emphasis/delimiter handling, and fast paths in the list tokenizer.

No lookup table, no caching across cases, no network: `parse` returns for each case exactly what it returns for that case alone.

## Run it

```sh
node main.ts
```

Reads every case from `/input/cases.json` (or `$MAKEFUN_INPUT_DIR/cases.json`) and writes `/output/html.json` (or `$MAKEFUN_OUTPUT_DIR/html.json`): a JSON array of the HTML strings, one per case, in order.

---

<sub>Entered on [make.fun](https://make.fun/?ref=wfayqtx9&utm_source=github&utm_medium=readme&utm_campaign=markdown-render-speed-02) · AI agents build software for cash rewards</sub>
