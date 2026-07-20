# report-to-brief

[![Version](https://img.shields.io/badge/version-0.2.0-blue.svg)](CHANGELOG.md)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
![Claude Code](https://img.shields.io/badge/Claude_Code-Skill-purple.svg)

[한국어](README.md) · **English**

A [Claude Code](https://claude.com/claude-code) skill that compresses long reports into **deployable policy briefs and executive summaries (1–8 pages)**.
Not a summarizer — **conclusion first**, **stands on its own**, **figures keep their sources**. Then it **checks the compressed text back against the original** and reports what went missing and what got bent.

<!-- demo GIF goes here -->

## A summary and a brief are not the same thing

|  | Summary | Brief |
|---|---|---|
| Order | Follows the original: intro → conclusion | **Conclusion first** |
| Assumed reader | Someone who read the original | **Someone who did not** |
| Figures | Sometimes kept, sometimes cut | Kept **with their sources** |
| Purpose | Convey content | **Support a decision** |

Those are formatting problems, and formatting problems are easy to fix. The hard part is that **compression causes accidents quietly** — key figures drop out, figures survive but their sources fall off, conditional claims harden into flat assertions. The brief itself looks fine, and **only someone who knows the original can catch it** — while the person reading the brief is, by definition, the person who did not read the original.

→ [Why I built this · detailed usage](docs/why.md)

## Three-axis compression review

```
Source → dissect (lock core assets) → ★compression-design gate → draft (conclusion-first) → three-axis review → brief + compression report
```

Going back and **comparing against the original** is the substance of this skill. Making a document shorter is not hard; **making it shorter without lying** is.

- **Axis 1 — fidelity.** Does every statement in the brief exist in the original? If not, 🔴 (hallucination during compression). Hedges are checked for survival too
- **Axis 2 — self-containment and conclusion-first.** Does it hold up for a reader who doesn't know the original? Is every acronym defined on first use?
- **Axis 3 — evidence preservation.** Did the figures bring their citations? A figure **the original itself never sourced** is not a hallucination, so it is flagged `⚠️[no source in original]` with an author query — no citation is invented

After the dissection the skill stops and shows you **"here is what I'll keep and what I'll cut"** — the cheapest point at which to change direction. The full specification is in [SKILL.md](SKILL.md), and a worked example running all four phases end to end is in [examples/example-brief.md](examples/example-brief.md) (a hypothetical 40-page report compressed to two pages).

## Install

```bash
mkdir -p ~/.claude/skills
cd ~/.claude/skills
git clone https://github.com/parkjui92/report-to-brief.git
```

If your install path takes a packaged bundle, use [`report-to-brief.skill`](report-to-brief.skill) directly.

## Usage

```
Compress this report into a 4-page policy brief. Audience: decision-makers   ← length + audience
Make this an executive summary. Conclusions, recommendations, risks only     ← it reads the type from your wording
Drop all the methodology, but keep the Japan case                            ← at the ★compression-design gate
Re-check axis 1. Paragraph 3 reads stronger than the original does           ← pushing back on the review
```

Length is `1p`/`2p`/`4p`/`8p` or "1/N of the original"; type is `policy_brief`, `exec_summary`, `one_pager`, or `issue_paper`; audience is decision-makers, practitioners, or general (defaults: `2p`, `policy_brief`, policy decision-makers). Prompts work in Korean or English.

## What you get

The brief body (`.md`) and a **compression report** — ratio, assets kept, three-axis results, and **what was deliberately excluded**. On request it converts to `.hwpx` or `.docx`, and **the original is never modified** (output always goes to a new file).
**Read the "deliberately excluded" line first.** Even with three green checks, if something you needed is on that list the brief failed — and that judgment is yours, not the review's.

## Limits

- **Compression is restatement.** It does not create claims, figures, or sources absent from the original — which also means **a weak original yields a weak brief**
- **The review is run by the skill itself.** It is not as independent as the agent-team kits, where reviewer and writer are separate agents. It reduces errors; it does not eliminate them
- **Source existence is not verified.** Axis 3 checks agreement with the original — whether the URL resolves and really contains that figure is [fact-verify](https://github.com/parkjui92/fact-verify)'s territory
- `.hwp`/`.docx`/`.pdf` input needs an extraction tool such as the [kordoc](https://github.com/chrisryugj/kordoc) MCP server (without it, paste the text and it proceeds). The length table is calibrated for Korean, and definitive page counts exist only after conversion

## Series

**Agent-team kits** — [policy-research-kit](https://github.com/parkjui92/policy-research-kit) (policy research reports) · [rnd-proposal-kit](https://github.com/parkjui92/rnd-proposal-kit) (Korean government R&D proposals) · [socsci-paper-kit](https://github.com/parkjui92/socsci-paper-kit) (social science papers)

**Authoring kit** — [lecture-deck-kit](https://github.com/parkjui92/lecture-deck-kit) (HTML lecture decks with in-browser live editing)

**Standalone skills** — **report-to-brief** (this repo, report compression) · [fact-verify](https://github.com/parkjui92/fact-verify) (source verification) · [paper-proofread](https://github.com/parkjui92/paper-proofread) (Korean academic proofreading) · [form-tailor](https://github.com/parkjui92/form-tailor) (institutional document formats)

## License

[MIT](LICENSE). The bundled example is built from a hypothetical report; no real client deliverables are included.
