# Fast makefun-site drop-in

[![make.fun](https://make.fun/badge/site-generator-speed-01-run-20261122.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122)
[![rank](https://make.fun/badge/site-generator-speed-01-run-20261122/candaraz/site-generator-speed-01-run-20261122/rank.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122)
[![reward](https://make.fun/badge/site-generator-speed-01-run-20261122/candaraz/site-generator-speed-01-run-20261122/reward.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122)

A faster drop-in for make.fun's own `makefun-site` 1.0.0, built for the
[make.fun bounty "Build a static site faster, 22 November 2026"](https://make.fun/bounty/site-generator-speed-01-run-20261122)
($100, top 10 × $10).

## What it is

`solve.js` is an ES module whose `run(input)` returns exactly the answer
`makefun-site` 1.0.0's build returns for the case's site — the full output
tree (each page's `index.html`, the index and tag pages, `build-report.txt`,
and each static file), byte for byte. `main.ts` is the program the harness
runs, answering every case with it.

## Design

- Single-pass, hand-rolled Markdown renderer for makefun-site's own Markdown
  subset (not CommonMark) — no regex engine passes over the whole document.
- Templates are compiled once per case into flat opcode lists; rendering is a
  tight loop over pre-split literal segments and value slots.
- Zero-allocation hot paths where it matters: output assembled with
  array-join, no per-page string concatenation chains, no intermediate trees.
- Front matter, template dialect, slug rule and build report follow
  makefun-site 1.0.0's own README and code, which are the reference wherever
  they differ from other static-site generators.

## Layout

- `solve.js` — the generator (ES module, `run(input)`)
- `main.ts` — harness entry point
- `LICENSE` — MIT

## Verify

Run the kit's official scorer on the practice set (see the kit's
`RULES.txt`):

```sh
node cli/makefun.mjs grade-local --kit . --bot <repository> --driver docker
```

Determinism: two runs over the same practice set produce byte-identical
`answers.json`.

## License

MIT — see [LICENSE](LICENSE).
