# report-to-brief

[![Version](https://img.shields.io/badge/version-0.2.0-blue.svg)](CHANGELOG.md)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
![Claude Code](https://img.shields.io/badge/Claude_Code-Skill-purple.svg)

[한국어](README.md) · **English**

> A [Claude Code](https://claude.com/claude-code) skill that compresses long reports into **deployable policy briefs and executive summaries (1–8 pages)**.
> Not a summarizer — it pulls the conclusion to the front, makes the brief stand on its own, carries figures along *with their sources*, and then **checks the compressed text back against the original to report what went missing and what got bent.**

---

## Why I built this

One of the most common requests in policy and research work is this: **"Can you cut this report down to four pages?"**

It feels ungenerous to whoever wrote the 200 pages, but the person asking has a point. **Decision-makers do not read the original.** Not out of indifference — there is more than one report on the desk. So the document that actually circulates, gets cited, and ends up justifying a decision is not the 200 pages. It is **those four pages**. The condensed version is the more consequential document.

But when you ask an LLM to "summarize this," what comes back is **a summary, not a brief.** The two differ more than they sound like they should.

- **A summary shrinks along the original's order.** It starts at the introduction and ends at the conclusion. A brief has to do the opposite — **the conclusion goes first**, and the reader has to be able to act on the opening paragraph alone. The assumption that a decision-maker will read as far as page three is usually wrong.
- **A summary writes sentences that lean on the original.** "This report examines…", "As discussed in Chapter 3…". A brief needs to be **self-contained**, because its reader is precisely the person who did not read that original.

Those are formatting problems, and formatting problems are easy to fix. The real problem is what comes next.

**Compression causes accidents quietly.** When you squeeze to fit a page target:

- key figures drop out entirely,
- figures survive but **their sources fall off**, and
- conditional claims harden into **flat assertions.** Hedges like "under certain conditions," "in the short term," or "this is a correlation and cannot be read as causal" are the first things that look like padding to whoever is cutting length. But that hedge is half the sentence. Remove it and the brief now says something the original never said.

The nasty part is that **only someone who knows the original can catch this.** The brief itself looks fine. The prose is smooth, the numbers are specific, the conclusion is crisp. Nothing surfaces unless you lay it next to the source. And the person reading the brief is, by definition, the person who did not read the source. **There is no signal inside the document telling them to check.**

So this skill doesn't stop at compressing. After compressing, it **goes back and compares against the original.** Does every statement in the brief actually exist in the source (axis 1)? Does it hold up for a reader who doesn't know the source (axis 2)? Did the figures bring their citations with them (axis 3)? That **three-axis compression review** is the substance of this skill. Making a document shorter is not hard. **Making it shorter without lying** is.

### The skill's own blind spot got caught once, too

Before release I ran a smoke test on a synthetic report and handed the output to a separate session — one with no knowledge of how it had been produced — for adversarial verification. That surfaced a blind spot in the skill itself: **figures that had no source in the original to begin with.** The review would flag "no source," and then deadlock against the rule that says never invent a citation. It caught the problem and had no move to make.

There is now a branch for it. That case is not the brief's fault — it is **a question to put back to the original**, so it is classified not as 🔴 (hallucination) but as `⚠️[no source in original]`, with a note asking the author to confirm where the figure came from. Attaching a plausible-looking citation would be the worst possible handling. The same round of verification also caught an internal contradiction in how length was measured (Markdown has no such thing as "page 2," yet compression ratios were being computed in pages). Full details are in [CHANGELOG.md](CHANGELOG.md).

---

## A summary and a brief are not the same thing

|  | Summary | Brief |
|---|---|---|
| Order | Follows the original: intro → conclusion | **Conclusion first** |
| Assumed reader | Someone who read the original | **Someone who did not** |
| Sentences | "This report examines…" | Statements that stand on their own |
| Figures | Sometimes kept, sometimes cut | Kept **with their sources** |
| Purpose | Convey content | **Support a decision** |

Here is what typically goes wrong in compression, and where this skill blocks it.

| Common compression failure | How this skill defends |
|---|---|
| **Inventing content** while compressing (hallucination) | **Axis 1** — every statement in the brief is checked against the original. Not there → 🔴 |
| Figures survive but **sources fall off** | **Axis 3** — key figures must carry their citation |
| Conditional claims become **overstated assertions** | **Axis 1** — hedges are checked for survival. The defense is also moved forward into drafting: "nuance, conditions, and sources are not compressible" |
| **Acronyms survive, definitions don't** | **Phase 1** — acronym/definition pairs are locked in as required assets (kept even when the definition sits in the introduction) |
| A **half-summary** that only makes sense if you know the original | **Axis 2** — self-containment and conclusion-first structure |
| Figures that **never had a source in the original** | **Axis 3 branch** — not a hallucination, so not 🔴. Flagged `⚠️[no source in original]` + author query. **No citation is invented** |

---

## How it works

```
[Source report]
   │
   ├─ Phase 1  Dissect — extract the assets that must survive, mark what goes
   │            └ ★ Lightweight gate: "compress it this way?"  ← you intervene here
   │
   ├─ Phase 2  Draft — rebuild on the chosen skeleton (conclusion-first wins)
   │
   ├─ Phase 3  Three-axis compression review — check against the original  ← the substance
   │            axis 1 fidelity · axis 2 self-containment · axis 3 evidence preservation
   │
   └─ Phase 4  Output — brief (.md) + compression report [+ .hwpx / .docx]
```

1. **Dissect** — conclusions, contested issues, recommendations, figures and their sources, acronym/definition pairs, and tables worth keeping are locked in as "core assets," and everything else is marked for cutting. Each figure is also tagged with **whether the original gave it a source**. Asset counts follow the report's actual structure rather than being forced into a fixed template ("five conclusions").
2. **Draft** — rebuilt on one of four skeletons: `policy_brief` (key message → background → issues → evidence → recommendations), `exec_summary`, `one_pager`, `issue_paper`. **Conclusion-first takes precedence over the skeleton's section order.**
3. **Three-axis review** — fidelity, self-containment, and evidence preservation are checked against the original; 🔴 and ⚠️ items are located and fixed.
4. **Output** — a Markdown brief plus a compression report. On request, converted to `.hwpx` or `.docx`.

The full specification is in [SKILL.md](SKILL.md). A worked example running all four phases end to end is in [examples/example-brief.md](examples/example-brief.md) — a hypothetical 40-page report compressed to two pages.

---

## Install

```bash
mkdir -p ~/.claude/skills
cd ~/.claude/skills
git clone https://github.com/parkjui92/report-to-brief.git
```

If your install path takes a packaged bundle, use [`report-to-brief.skill`](report-to-brief.skill) directly.

Restart Claude Code and it will pick up requests like "turn this into a brief," "executive summary," "get it down to one page," or the Korean equivalents. Prompts work in either language.

> **A note on the Korean context.** The default output shape is a *정책브리프* (policy brief) — in Korean policy practice, a short standalone document circulated to decision-makers ahead of a meeting, and often the only version anyone reads. Its longer sibling, the *이슈페이퍼* (issue paper), runs to roughly eight pages and adds analysis. Neither is exotic: they map closely onto the policy brief and executive summary formats used everywhere else, which is why the skill's four skeletons are named in those terms.

---

## How to use it

### Scenario 1 — A long report into a policy brief

The basic case. Attach the file and say:

```
Compress the attached policy research report into a 4-page policy brief.
Audience is director-level decision-makers.
```

Give it both length *and* audience. Once the audience is fixed, the keep/cut boundary has a criterion — for a practitioner audience, methodology and process survive; for decision-makers, that space goes to recommendations and risks instead.

### Scenario 2 — One page to carry into the room

```
Make this an executive summary. Conclusions, recommendations, and risks only.
```

```
One page. Three to five key messages and an evidence box, that's it.
```

The first uses the `exec_summary` skeleton (conclusions and recommendations → key evidence → risks and next steps), the second `one_pager`. You don't have to name the type — it reads the intent from the request.

### Scenario 3 — When figures and sources must survive

```
Make it an issue paper, but keep every figure and its source.
```

This is actually the default behavior. The skill is built so that **if a figure survives, its citation survives with it**, and hedges, conditions, and sources are never deleted to hit a length target — length comes out of secondary examples and repeated explanation instead. Saying it explicitly still makes the review report more granular.

### Scenario 4 — Compressing a report you already finished

If you produced a 50-page policy research report with the sister repo [policy-research-kit](https://github.com/parkjui92/policy-research-kit), hand the result straight to this skill.

```
Cut the report I just produced down to a 4-page brief
```

**This is a different job from the kit's own brief mode.** The kit's brief mode writes a brief *as the research deliverable* from the start. This skill **compresses a document that already exists**. If you have a finished original and need it shorter, this is the right tool; if you are writing from a blank page, it isn't.

### Specifying length

Markdown has no fixed "page." So the skill manages length **by character count** and computes the compression ratio on the same basis. Actual page counts are reported only after conversion to `.hwpx` or `.docx`.

| Target | Body length (Korean characters) | Fitting type |
|--------|--------------------------------|--------------|
| `1p` | ~1,600 | one_pager |
| `2p` | ~3,200 | policy_brief standard (default) |
| `4p` | ~6,400 | |
| `8p` | ~12,800 | issue_paper ceiling |

This table is calibrated for **Korean-language body text**. Working in another language, express the target as a ratio instead — `"one-fifth of the original"` is a supported way to ask.

Either way, when the ratio gets aggressive (below about 1/20), the only things that can survive are conclusions and recommendations. At that point you'll get a better result by **narrowing what the brief is for** than by asking for more pages.

### ★ Intervening at the compression-design gate

At the end of Phase 1 the skill stops and shows you **"here is what I'll keep and what I'll cut."** This is **the cheapest point at which to change direction** — far better than discovering the omission after the brief is written. Just say so:

```
Drop all the methodology, but keep the Japan case
Keep only the supply-demand table
Leave all three recommendations, cut the background in half
Chapter 2's issues are the point of this brief — give them the space
```

Agreement on what lives and what dies drives compression quality more than the drafting does. Most unsatisfying briefs are decided at this step, not the next one.

> In non-interactive contexts (batch runs, subagents) where a round-trip is impossible, it does not halt. It **states the compression design and its assumptions in the report** and proceeds.

### Reading the three-axis review

The brief comes with a report like this. It is half the reason to use the skill.

```
## Compression summary
- Source: (filename), ~M chars → brief: K chars (ratio 1/x, by character count)
  · page count appended after hwpx/docx conversion
- Type: policy_brief | Audience: decision-makers
- Assets kept: conclusions/issues/recommendations n / key figures n (sourced n, [no source in original] n) / acronym definitions n / tables n
- Three-axis review: fidelity ✅ | self-containment ✅ | evidence ⚠️ 1 ([no source in original] → confirm with author)
- Deliberately excluded: methodology detail, 2 of 3 international cases, appendix
```

**Read the last line first — "deliberately excluded."** Even with three green checks, if something you needed is on that list, the brief failed. The review can tell you whether something was dropped; it cannot tell you whether it was *safe* to drop. That judgment is yours.

The verdict marks read as follows.

| Mark | Meaning | What you do |
|---|---|---|
| ✅ | No problem on that axis | — |
| 🔴 | The brief contains a statement absent from the original = **hallucination during compression** | Must be fixed. The skill points to the location |
| ⚠️ `[no source in original]` | A figure **the original itself** left uncited | Not a defect in the brief — it's **a query about the source document**. Ask the original author where it came from |
| `rearranged from source figures` | A table built from the original's numbers where the original had none | No new fact was added. Fine to leave as is |

On axis 3 the skill separates two cases that are easy to confuse. **They get opposite treatment.**

- **Lost in compression** — the original had a citation and the brief dropped it. → **Restore the original's citation.** Legitimate fix.
- **Never sourced in the original** — the original never cited it either. → Mark "(uncited in original)" in the brief and ask the author to confirm. **Do not invent a source.** Baseless attributions like "author's estimate" are equally forbidden.

If the review doesn't convince you, push back:

```
Re-check axis 1. Paragraph 3 reads stronger than the original does
Tell me which page of the original this figure came from
```

### What inputs it accepts

| Input | Handling | Without the tool |
|---|---|---|
| `.hwp` / `.hwpx` | Text and tables extracted via the [kordoc](https://github.com/chrisryugj/kordoc) MCP server | Paste the text and it proceeds |
| `.docx` | pandoc or python-docx | 〃 |
| `.pdf` | pdf skill | 〃 |
| `.md` · pasted text | Used directly | — |

> HWP/HWPX is Hangul Word Processor format — the de facto standard for Korean government and institutional documents, and the reason first-class support for it matters here. Note that text extraction from `.hwp` can flatten tables and figures. If the original is table-heavy, check the extraction before starting, and name any table that must survive at the compression-design gate.

**To verify that the sources themselves are real**, install [fact-verify](https://github.com/parkjui92/fact-verify) alongside it and the two connect. Axis 3 here checks agreement with the original; it does not check whether the cited source exists and actually contains that figure.

---

## What you get

| Output | Contents |
|---|---|
| Brief body (`.md`) | Conclusion-first brief matching the chosen type, length, and audience |
| Compression report | Ratio, assets kept, **three-axis review results**, what was deliberately excluded |
| (on request) `.hwpx` / `.docx` | Distribution-ready conversion. For institutional templates, connect [form-tailor](https://github.com/parkjui92/form-tailor) |

The governing principle is that every key item in the brief stays **traceable back to where it came from in the original**. Which means that when someone in the meeting asks where a figure came from, you can answer.

**The original is never modified.** Output always goes to a new file.

---

## Options

| Option | Default | Values |
|--------|---------|--------|
| Length | `2p` | `1p` / `2p` / `4p` / `8p`, or "1/N of the original" |
| Type | `policy_brief` | `policy_brief` / `exec_summary` / `one_pager` / `issue_paper` |
| Audience | Policy decision-makers | Decision-makers / practitioners / general |

---

## Scope and limitations

- **Compression is restatement.** It does not create claims, figures, or sources absent from the original. Which also means: **a weak original yields a weak brief.** This skill does not manufacture insight that wasn't there.
- **The review is run by the skill itself.** It checks its own compression against the source, so it is not as independent as the agent-team kits, where reviewer and writer are separate agents. It reduces errors; it does not eliminate them. Final responsibility stays with a human.
- **Source existence is not verified.** Axis 3 checks agreement with the original. Whether the URL resolves and whether that document really contains the figure is [fact-verify](https://github.com/parkjui92/fact-verify)'s territory.
- **The length table is calibrated for Korean**, and definitive page counts exist only after `.hwpx`/`.docx` conversion.
- **The bundled example is built from a hypothetical report.** No real client deliverables are included.
- Writing from a blank page, and conversion into institutional templates, are other skills' jobs.

---

## Series

Sister repositories built on the same design philosophy.

**Agent-team kits** — [policy-research-kit](https://github.com/parkjui92/policy-research-kit) (policy research reports) · [rnd-proposal-kit](https://github.com/parkjui92/rnd-proposal-kit) (Korean government R&D proposals) · [socsci-paper-kit](https://github.com/parkjui92/socsci-paper-kit) (social science papers)

**Standalone skills** — **report-to-brief** (this repo, report compression) · [fact-verify](https://github.com/parkjui92/fact-verify) (source verification) · [paper-proofread](https://github.com/parkjui92/paper-proofread) (Korean academic proofreading) · [form-tailor](https://github.com/parkjui92/form-tailor) (institutional document formats)

Chained together: **write the report with a kit → compress it with report-to-brief → fit it to an institutional template with form-tailor → check the sources with fact-verify.**

---

## License

[MIT](LICENSE)
