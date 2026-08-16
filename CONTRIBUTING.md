# Contributing

This repo **is** the field index. Cheap corrections beat long essays. Add sources, not essays.

## What belongs here

Public ecosystem facts: layers, vendors, APIs, prices, papers, comparisons.

**Out of scope:** unreleased product architecture, private roadmaps, leaked keys, “please add my startup” with no layer fit.

One chapter = one concern. Do not mash relatedness + intent + compacting into one file.

## Fix a fact

1. Open the chapter in `book/en/` **and** `book/zh/` (same filename).
2. Patch the smallest block (a cell, a multiplier, a sentence).
3. In the PR: official URL + retrieval date (YYYY-MM-DD).
4. **Numbers need a source:** Anthropic cache read 0.1×, Batch 50%, Codex compact at 90% window, Flex discounts, RouteLLM %, CUA benchmark scores.

If you only speak one language: update that side and check `translation-lag` in the PR template so someone else can sync.

## Add a paper / repo / official doc

1. Add a **Read** line on the chapter: one sentence (what the source claims) + URL. Mark paper / repo / docs in that sentence.
2. Add a short note to `book/en/bibliography.md` and `book/zh/bibliography.md` (union of all chapter links).
3. Prefer primary sources: official docs, arXiv abs, original GitHub, lab engineering posts. Skip blogspam.



## Add a chapter / layer

Open `missing-layer` or `new-chapter` first. New files must exist in **both** `book/en/` and `book/zh/` with the same path. Follow the chapter shape:

1. One-line what it is
2. **Detailed concept** — definition (is / is not), why it exists, mechanism, boundaries (what it is easily confused with), concrete comparison or example, failure modes
3. Comparison table if it helps
4. **Read**  — other people's papers / repos / official docs; one-sentence intro + URL each. Do not replace the concept with links.



## Style

- Detailed concepts, no filler, no repeating the same sentence three ways.
- This is an industry field index, not a programming tutorial.
- EN: English body, Chinese gloss on jargon.
- ZH: Chinese body, English proper nouns (Scale AI, LiteLLM, SWE-bench, Mamba, MCP).
- Do not fork a second architecture. If two chapters disagree, fix the older one.



## License

By contributing you license text under [CC-BY-4.0](./LICENSE).