# report-to-brief

[![Version](https://img.shields.io/badge/version-0.2.0-blue.svg)](CHANGELOG.md)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
![Claude Code](https://img.shields.io/badge/Claude_Code-Skill-purple.svg)

[한국어](README.md) · **English**

A [Claude Code](https://claude.com/claude-code) skill that turns a long report into a short one — 1 to 8 pages.

Hand it a report and it moves the conclusion to the front, rewrites it so the short version makes sense on its own, and keeps the important numbers with their sources still attached. Then it goes back through the original and tells you **what dropped out and what got bent** along the way.

## A summary and a brief are not the same thing

Ask any AI to "summarize this" and you get a **summary**. That is not a brief, and the two differ more than people expect.

|  | Summary | Brief |
|---|---|---|
| Order | Follows the original, start to finish | **Conclusion comes first** |
| Written for | Someone who read the original | **Someone who did not** |
| Numbers | Sometimes kept, sometimes cut | Always kept **with their sources** |
| Purpose | To pass content along | **To support a decision** |

Reordering is the easy part. The hard part is that **shortening causes accidents quietly.** A key number disappears. A number survives but its citation falls off. A claim the original made only "under certain conditions" loses that condition and reads as flat fact.

None of this is visible from the brief alone. **Only someone who knows the original can catch it** — and the person reading a brief is, by definition, the person who did not read the original. So once it's short, everything gets compared back against the source.

→ [Why I built this, and fuller usage notes](docs/why.md) · [Full specification](SKILL.md)

## How it runs

```
Read the source → pick what to keep → ★You confirm → Write it short → Compare back → Brief + comparison report
```

It **stops once.** After reading the original, and before writing anything, it shows you "here's what I'll keep and what I'll cut." Say "drop all the methodology" and it does. **It's the cheapest moment to change direction.**

## What it checks

Making a document shorter is not hard. **Making it shorter without lying** is. So once the short version exists, it goes back to the original and checks three things.

- **Is this actually in the original?** Every sentence in the brief gets traced back. If it isn't there, it's flagged 🔴 — something appeared that was never said. Conditions like "only when…" are checked for survival too
- **Does it make sense on its own?** Read it as if you'd never seen the original: does it still hold up? Is every acronym spelled out the first time it appears?
- **Did the numbers bring their sources?** Important figures are checked for citations. A number **the original itself never sourced** isn't the skill's invention, so it gets marked `⚠️[no source in original]` and handed back for you to confirm. No citation is ever made up to fill the gap

## Install

```bash
mkdir -p ~/.claude/skills
cd ~/.claude/skills
git clone https://github.com/parkjui92/report-to-brief.git
```

If your install path takes a packaged bundle, use [`report-to-brief.skill`](report-to-brief.skill) directly.

## Using it

Just ask in plain language.

```
Compress this report into a 4-page policy brief. Audience: decision-makers   ← length + who reads it
Make this an executive summary. Conclusions, recommendations, risks only     ← it reads the type from your wording
Drop all the methodology, but keep the Japan case                            ← at the ★confirmation step
Re-check the first one. Paragraph 3 reads stronger than the original does    ← when a check looks off
```

Length is `1p`/`2p`/`4p`/`8p` or "1/N of the original"; the document type is `policy_brief`, `exec_summary`, `one_pager`, or `issue_paper`; the reader is decision-makers, practitioners, or general. Set none of it and you get `2p`, `policy_brief`, decision-makers. Prompts work in Korean or English.

## What you end up with

The brief itself (`.md`) and a **comparison report** — how much it shrank, what was kept, how the three checks came out, and **what was deliberately left out**. On request it also converts to `.docx` or `.hwpx` (the document format Korean government offices use), and **your original file is never touched** — output always goes somewhere new.

**Read the "deliberately left out" line first.** Even with three green checks, if something you needed is on that list, the brief failed. That call is yours to make, not the skill's.

## Good to know

- **Shortening is restatement.** It won't invent claims, numbers, or sources the original doesn't have — which also means **a weak original gives you a weak brief**
- **The skill checks its own work.** That's less independent than the agent-team kits, where the writer and the reviewer are separate AIs. It reduces errors; it doesn't eliminate them
- **It doesn't check whether a source is real.** The third check only confirms the brief matches the original. Whether the link still resolves, and whether that document really contains that number, is [fact-verify](https://github.com/parkjui92/fact-verify)'s job
- Feeding it a `.hwp`, `.docx`, or `.pdf` directly needs a separate extraction tool such as the [kordoc](https://github.com/chrisryugj/kordoc) MCP server. Without one, paste the text in and it proceeds as normal
- The length math counts Korean characters, and **the exact page count is only knowable after the file is converted**

## Related work

**Plugins that write reports and proposals** — [policy-research-kit](https://github.com/parkjui92/policy-research-kit) (policy research reports) · [rnd-proposal-kit](https://github.com/parkjui92/rnd-proposal-kit) (Korean government R&D proposals) · [socsci-paper-kit](https://github.com/parkjui92/socsci-paper-kit) (social science papers)

**Plugins that build and edit** — [lecture-deck-kit](https://github.com/parkjui92/lecture-deck-kit) (HTML lecture slides you edit right in the browser)

**Single-purpose tools** — [fact-verify](https://github.com/parkjui92/fact-verify) (check whether sources are real) · [paper-proofread](https://github.com/parkjui92/paper-proofread) (Korean academic proofreading) · [form-tailor](https://github.com/parkjui92/form-tailor) (match an organization's document format) · **report-to-brief** (this repository)

## License

[MIT](LICENSE). No real client deliverables are included.
