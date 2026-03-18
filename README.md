# Auto-claude-code-research-in-sleep (ARIS ⚔️🌙)

![ARIS Logo](docs/aris_logo.svg)

![Hero](docs/hero_combined.svg)

[中文版 README](README_CN.md) | English

![Score Progression](docs/auto_review_score_curve.png)

> 🌙 **Let Claude Code do research while you sleep.** Wake up to find your paper scored, weaknesses identified, experiments run, and narrative rewritten — autonomously.
>
> 🪶 **Radically lightweight — zero dependencies, zero lock-in.** The entire system is plain Markdown files. No framework to learn, no database to maintain, no Docker to configure, no daemon to babysit. Every skill is a single `SKILL.md` readable by any LLM — swap Claude Code for [OpenClaw](docs/OPENCLAW_ADAPTATION.md), [Cursor](docs/CURSOR_ADAPTATION.md), Windsurf, or your own agent and the workflows still work. Fork it, rewrite it, adapt it to your stack.
>
> *💡 ARIS is a methodology, not a platform. What matters is the research workflow — take it wherever you go. 🌱*

[![Featured on PaperWeekly](https://img.shields.io/badge/Featured%20on-PaperWeekly-red?style=flat)](https://mp.weixin.qq.com/s/tDniVryVGjDkkkWl-5sTkQ) · [![Featured in awesome-agent-skills](https://img.shields.io/badge/Featured%20in-awesome--agent--skills-blue?style=flat&logo=github)](https://github.com/VoltAgent/awesome-agent-skills) · [![AI Digital Crew - Project of the Day](https://img.shields.io/badge/AI%20Digital%20Crew-Project%20of%20the%20Day%20(2026.03.14)-orange?style=flat)](https://aidigitalcrew.com) · [💬 Join Community](#-community) · [![Cite](https://img.shields.io/badge/📖_Cite_Us-BibTeX-green?style=flat)](#-citation)

Custom [Claude Code](https://docs.anthropic.com/en/docs/claude-code) skills for autonomous ML research workflows. These skills orchestrate **cross-model collaboration** — Claude Code drives the research while an external LLM (via [Codex MCP](https://github.com/openai/codex)) acts as a critical reviewer. 🔀 **Also supports [alternative model combinations](#-alternative-model-combinations) (GLM, MiniMax, Kimi, LongCat, DeepSeek, etc.) — no Claude or OpenAI API required.** 🤖 **[Codex CLI native](skills/skills-codex/)** — full skill set also available for OpenAI Codex. 🖱️ **[Cursor](docs/CURSOR_ADAPTATION.md)** — works in Cursor too. 🆓 **[Free tier via ModelScope](docs/MODELSCOPE_GUIDE.md) — zero cost, zero lock-in.**

> 💭 **Why not self-play with a single model?** Using Claude Code subagents or agent teams for both execution and review is technically possible, but tends to fall into **local minima** — the same model reviewing its own patterns creates blind spots.
>
> *Think of it like adversarial vs. stochastic bandits: a single model self-reviewing is the stochastic case (predictable reward noise), while cross-model review is adversarial (the reviewer actively probes weaknesses the executor didn't anticipate) — and adversarial bandits are fundamentally harder to game.*
>
> 💭 **Why two models, not more?** Two is the minimum needed to break self-play blind spots, and 2-player games converge to Nash equilibrium far more efficiently than n-player ones. Adding more reviewers increases API cost and coordination overhead with diminishing returns — the biggest gain is going from 1→2, not 2→4.
>
> Claude Code's strength is fast, fluid execution; Codex (GPT-5.4 xhigh) is slower but more deliberate and rigorous in critique. These complementary styles — **speed × rigor** — produce better outcomes than either model talking to itself.

## 📢 What's New

- **2026-03-18** — ![NEW](https://img.shields.io/badge/NEW-red?style=flat-square) 🎤 **[`paper-slides`](skills/paper-slides/SKILL.md)** — Conference presentation slides (beamer → PDF + PPTX) with speaker notes, talk script, and Q&A prep. 4 talk types (oral/spotlight/poster/invited). Community contribution by [@dengzhe-hou](https://github.com/dengzhe-hou)
- **2026-03-18** — ![NEW](https://img.shields.io/badge/NEW-red?style=flat-square) 🔁 **[Codex + Claude reviewer bridge](docs/CODEX_CLAUDE_REVIEW_GUIDE.md)** — Codex executes, Claude reviews via local `claude-review` MCP bridge. Community contribution by [@loujc](https://github.com/loujc)
- **2026-03-18** — ![NEW](https://img.shields.io/badge/NEW-red?style=flat-square) 🖱️ **[Cursor adaptation guide](docs/CURSOR_ADAPTATION.md)** — use ARIS skills in [Cursor](https://www.cursor.com/) with `@`-reference, MCP setup, and state file recovery. Community contribution by [@YecanLee](https://github.com/YecanLee)
- **2026-03-18** — ![NEW](https://img.shields.io/badge/NEW-red?style=flat-square) 🤖 **[Codex CLI native skills](skills/skills-codex/)** — full 31-skill ARIS set for [Codex CLI](https://github.com/openai/codex) using `spawn_agent`. Community contributions by [@Falling-Flower](https://github.com/Falling-Flower) & [@No-518](https://github.com/No-518)
- **2026-03-18** — ![NEW](https://img.shields.io/badge/NEW-red?style=flat-square) 📝 **[`grant-proposal`](skills/grant-proposal/SKILL.md)** — Draft structured grant proposals from research ideas. Supports 9 agencies: KAKENHI (Japan), NSF (US), NSFC (China, incl. 面上/青年/优青/杰青/海外优青/重点), ERC (EU), DFG, SNSF, ARC, NWO, and generic. Chains `/research-lit` → `/novelty-check` → `/research-review` → `/paper-illustration`. Community contribution by [@dengzhe-hou](https://github.com/dengzhe-hou)
- **2026-03-18** — ![NEW](https://img.shields.io/badge/NEW-red?style=flat-square) 🎨 **[`paper-illustration`](skills/paper-illustration/SKILL.md)** — AI-generated publication-quality architecture diagrams and method figures. Claude plans → Gemini renders → iterative refinement until score ≥ 9. Integrated into Workflow 3 (`illustration: true`, requires `GEMINI_API_KEY`). Built on [PaperBanana](https://github.com/dwzhu-pku/PaperBanana). Community contribution by [@Joseph-li343](https://github.com/Joseph-li343)
  <details><summary>Preview demo</summary><br><img src="assets/paper_illustration_demo.png" width="600" alt="paper-illustration demo" /></details>
- **2026-03-18** — ![NEW](https://img.shields.io/badge/NEW-red?style=flat-square) 📊 **W&B integration** — auto `wandb.log()` when `wandb: true`. 🔗 **[Workflow 1.5](skills/experiment-bridge/SKILL.md)** — `/experiment-bridge`: plan → implement → deploy → collect
- **2026-03-18** — ![NEW](https://img.shields.io/badge/NEW-red?style=flat-square) 📊 **[CitationClaw](https://github.com/VisionXLab/CitationClaw)** — citation impact analysis: paper title → citation crawling, scholar identification, tiered analysis, HTML dashboard
- **2026-03-17** — ![NEW](https://img.shields.io/badge/NEW-red?style=flat-square) 🔧 **Git code sync** — `/run-experiment` now supports `code_sync: git` (`git push` → `ssh "git pull"`) as alternative to rsync. **[NARRATIVE_REPORT example](docs/NARRATIVE_REPORT_EXAMPLE.md)** for Workflow 3. **Parameter pass-through** — set any downstream parameter at any level with `— key: value` ([details](#%EF%B8%8F-customization)). 🆓 **[ModelScope guide](docs/MODELSCOPE_GUIDE.md)** — free (2000 calls/day), one key, no automation restrictions ([Alt E](#-alternative-model-combinations))
<details>
<summary>Earlier updates (2026-03-12 — 2026-03-16)</summary>

- **2026-03-16** — 🔬 **[`research-refine`](skills/research-refine/SKILL.md)** + [`experiment-plan`](skills/experiment-plan/SKILL.md) — turn vague ideas into problem-anchored proposals with claim-driven experiment roadmaps. Now integrated into Workflow 1 (`/idea-discovery`). Community contribution by [@zjYao36](https://github.com/zjYao36)
- **2026-03-16** — 🇨🇳 **[Alibaba Coding Plan guide](docs/ALI_CODING_PLAN_GUIDE.md)** — one API key, 4 models (Kimi-K2.5 + Qwen3.5+ + GLM-5 + MiniMax-M2.5), dual-endpoint setup. Community contribution by [@tianhao909](https://github.com/tianhao909)
- **2026-03-15** — 🔀 **Bring your own model!** [Any OpenAI-compatible API](#-alternative-model-combinations) now works as reviewer via [`llm-chat`](mcp-servers/llm-chat/) MCP server. GLM, MiniMax, Kimi, LongCat, DeepSeek all tested — **zero Claude or OpenAI API needed**
- **2026-03-15** — 🐾 **[OpenClaw adaptation guide](docs/OPENCLAW_ADAPTATION.md)** — use ARIS research workflows in [OpenClaw](https://github.com/All-Hands-AI/OpenHands) without Claude Code slash skills
- **2026-03-15** — 📐 **[`proof-writer`](skills/proof-writer/SKILL.md)** — community skill for rigorous theorem proof drafting. 📚 **Anti-hallucination citations** — `/paper-write` now fetches real BibTeX from [DBLP](https://dblp.org)/[CrossRef](https://www.crossref.org) instead of LLM-generated entries — on by default, zero install
- **2026-03-14** — 📱 [Feishu/Lark integration](#-feishulark-integration-optional): three modes (off/push/interactive), mobile notifications for experiments, reviews, and checkpoints
- **2026-03-13** — 🛑 Human-in-the-loop: configurable `AUTO_PROCEED` checkpoints across all workflows. Full autopilot or step-by-step approval
- **2026-03-12** — 🔗 [Zotero](#-zotero-integration-optional) + [Obsidian](#-obsidian-integration-optional) + local PDFs + arXiv/Scholar: multi-source literature search with cross-model novelty verification
- **2026-03-12** — 🚀 Three end-to-end workflows complete: one prompt → top-venue-style paper. `/research-pipeline` chains idea discovery → auto review → paper writing autonomously
- **2026-03-12** — 📝 `/paper-writing` workflow: narrative report → structured outline → figures → LaTeX → compiled PDF → 2-round auto-improvement (4/10 → 8.5/10)

</details>

## 🚀 Quick Start

```bash
# 1. Install skills
git clone https://github.com/wanshuiyin/Auto-claude-code-research-in-sleep.git
cp -r Auto-claude-code-research-in-sleep/skills/* ~/.claude/skills/

# 2. Set up Codex MCP (for review skills)
npm install -g @openai/codex
codex setup                    # set model to gpt-5.4 when prompted
claude mcp add codex -s user -- codex mcp-server

# 3. Use in Claude Code
claude
> /idea-discovery "your research direction"  # Workflow 1 — be specific! not "NLP" but "factorized gap in discrete diffusion LMs"
> /experiment-bridge                         # Workflow 1.5 — have a plan? implement + deploy + collect results
> /auto-review-loop "your paper topic or scope"  # Workflow 2: review → fix → re-review overnight
> /paper-writing "NARRATIVE_REPORT.md"       # Workflow 3: narrative → polished PDF
> /research-pipeline "your research direction"  # Full pipeline: Workflow 1 → 1.5 → 2 → 3 end-to-end
```

> 📝 **New to Workflow 3?** See [`docs/NARRATIVE_REPORT_EXAMPLE.md`](docs/NARRATIVE_REPORT_EXAMPLE.md) for a complete sample input — shows what claims, experiments, results, and figure descriptions to include for best results.

> **Tip:** All pipeline behaviors are configurable via inline overrides — append `— key: value` to any command:
>
> | Parameter | Default | What it does |
> |-----------|---------|-------------|
> | `AUTO_PROCEED` | `true` | Auto-continue at idea selection gate. Set `false` to manually pick which idea to pursue before committing GPU time |
> | `human checkpoint` | `false` | Pause after each review round so you can read the score, give custom modification instructions, skip specific fixes, or stop early |
> | `sources` | `all` | Which literature sources to search: `zotero`, `obsidian`, `local`, `web`, or `all` (comma-separated) |
> | `arxiv download` | `false` | Download top relevant arXiv PDFs during literature survey. When `false`, only fetches metadata (title, abstract, authors) |
> | `DBLP_BIBTEX` | `true` | Fetch real BibTeX from [DBLP](https://dblp.org)/[CrossRef](https://www.crossref.org) instead of LLM-generated entries. Eliminates hallucinated citations. Zero install |
> | `code review` | `true` | GPT-5.4 xhigh reviews experiment code before GPU deployment. Set `false` to skip |
> | `wandb` | `false` | Auto-add W&B logging to experiment scripts. Set `true` + configure `wandb_project` in CLAUDE.md. `/monitor-experiment` pulls training curves from W&B |
> | `illustration` | `false` | Auto-generate architecture/method diagrams via Gemini in Workflow 3. Requires `GEMINI_API_KEY` env var |
>
> ```
> /research-pipeline "your topic" — AUTO_PROCEED: false                          # pause at idea selection gate
> /research-pipeline "your topic" — human checkpoint: true                       # pause after each review round to give feedback
> /research-pipeline "your topic" — sources: zotero, web                         # only search Zotero + web (skip local PDFs)
> /research-pipeline "your topic" — arxiv download: true                         # download top arXiv PDFs during literature survey
> /research-pipeline "your topic" — AUTO_PROCEED: false, human checkpoint: true  # combine options
> ```

> **Important:** Codex MCP uses the model from `~/.codex/config.toml`, not from skill files. Make sure it says `model = "gpt-5.4"` (recommended). Other options: `gpt-5.3-codex`, `gpt-5.2-codex`, `o3`. Run `codex setup` or edit the file directly.

> **Want Codex to execute but Claude Code to review?** See [`docs/CODEX_CLAUDE_REVIEW_GUIDE.md`](docs/CODEX_CLAUDE_REVIEW_GUIDE.md). That path installs the base `skills/skills-codex/*`, then overlays `skills/skills-codex-claude-review/*`, and routes review-heavy skills through the local `claude-review` MCP bridge.

See [full setup guide](#%EF%B8%8F-setup) for details and [alternative model combinations](#-alternative-model-combinations) if you don't have Claude/OpenAI API.

## ✨ Features

- 📊 **31 composable skills** — mix and match, or chain into full pipelines (`/idea-discovery`, `/auto-review-loop`, `/paper-writing`, `/research-pipeline`)
- 🔍 **Literature & novelty** — multi-source paper search (**[Zotero](#-zotero-integration-optional)** + **[Obsidian](#-obsidian-integration-optional)** + **local PDFs** + arXiv/Scholar) + cross-model novelty verification
- 💡 **Idea discovery** — literature survey → brainstorm 8-12 ideas → novelty check → GPU pilot experiments → ranked report
- 🔄 **Auto review loop** — 4-round autonomous review, 5/10 → 7.5/10 overnight with 20+ GPU experiments
- 📝 **Paper writing** — narrative → outline → figures → LaTeX → PDF → auto-review (4/10 → 8.5/10), one command. Anti-hallucination citations via [DBLP](https://dblp.org)/[CrossRef](https://www.crossref.org)
- 🤖 **Cross-model collaboration** — Claude Code executes, GPT-5.4 xhigh reviews. Adversarial, not self-play
- 📝 **Peer review** — review others' papers as a conference reviewer, with structured scoring and meta-review
- 🖥️ **Review-driven experiments** — when GPT-5.4 says "run an ablation", Claude Code automatically writes the script, rsyncs to your GPU server, launches in screen, collects results, and folds them back into the paper. Just configure your server in `CLAUDE.md` ([setup guide](#%EF%B8%8F-gpu-server-setup-for-auto-experiments))
- 🔀 **Flexible models** — default Claude × GPT-5.4, also supports [GLM, MiniMax, Kimi, LongCat, DeepSeek, etc.](#-alternative-model-combinations) — no Claude or OpenAI API required
- 🛑 **Human-in-the-loop** — configurable checkpoints at key decisions. `AUTO_PROCEED=true` for full autopilot, `false` to approve each step
- 📱 **[Feishu/Lark notifications](#-feishulark-integration-optional)** — three modes: **off (default, strongly recommended for most users)**, push-only (webhook, mobile alerts), interactive (approve/reject from Feishu). Zero impact when unconfigured

  <details>
  <summary>Preview: Push cards (group) &amp; Interactive chat (private)</summary>

  **Push Only** — group chat cards (experiment done, checkpoint, error, pipeline complete):

  <img src="assets/feishu_push.png" width="700" />

  **Interactive** — private chat with Claude Code (approve/reject, custom instructions):

  <img src="assets/feishu_interactive.jpg" width="700" />

  </details>

- 🧩 **Extensible** — domain-specific skills welcome! Add a `SKILL.md` and open a PR. See [community skills](#-all-skills) like [`dse-loop`](skills/dse-loop/SKILL.md) (architecture/EDA)

---

## 📈 Score Progression (Real Run)

A real overnight 4-round run on an ML research project, from borderline reject to submission-ready:

| Round | Score | What Happened |
|-------|-------|---------------|
| Initial | 5.0/10 | Borderline reject |
| Round 1 | 6.5/10 | Added standard metrics, discovered metric decoupling |
| Round 2 | 6.8/10 | Key claim failed to reproduce, pivoted narrative |
| Round 3 | 7.0/10 | Large seed study killed main improvement claim |
| Round 4 | **7.5/10** ✅ | Diagnostic evidence solidified, **submission ready** |

The loop autonomously ran **20+ GPU experiments**, rewrote the paper's narrative framing, and killed claims that didn't hold up — all without human intervention.

## 🧩 Awesome Community Skills & Extensions

Domain-specific skills and external projects contributed by the community. PRs welcome — just add a `skills/your-skill/SKILL.md` and open a PR!

> 💡 **How to use:** Community skills are not auto-wired into core workflows. To use one, ask your executor (Claude Code / OpenClaw / etc.) to read the skill's `SKILL.md`, then plug it into the appropriate workflow stage based on the description below.

🎉 **Community Skills (9):** [research-refine](skills/research-refine/SKILL.md) · [experiment-plan](skills/experiment-plan/SKILL.md) · [grant-proposal](skills/grant-proposal/SKILL.md) · [paper-slides](skills/paper-slides/SKILL.md) · [proof-writer](skills/proof-writer/SKILL.md) · [comm-lit-review](skills/comm-lit-review/SKILL.md) · [dse-loop](skills/dse-loop/SKILL.md) · [idea-discovery-robot](skills/idea-discovery-robot/SKILL.md) · [paper-illustration](skills/paper-illustration/SKILL.md)

🌐 **External Projects & Docs (6):** [open-source-hardening-skills](https://github.com/zeyuzhangzyz/open-source-hardening-skills) · [CitationClaw](https://github.com/VisionXLab/CitationClaw) · [OpenClaw Adaptation Guide](docs/OPENCLAW_ADAPTATION.md) · [Cursor Adaptation Guide](docs/CURSOR_ADAPTATION.md) · [Codex+Claude Review Bridge](docs/CODEX_CLAUDE_REVIEW_GUIDE.md) · [paper-illustration](skills/paper-illustration/SKILL.md)

> 🙌 Thanks to every contributor! We fold the tables below to keep the README readable — but every skill and project here is equally valued. PRs always welcome!

<details>
<summary><b>🎉 Community Skills (9)</b> — click to expand</summary>

| Name | Domain | Description | Codex MCP? |
|------|--------|-------------|-----------|
| 🔬 [`research-refine`](skills/research-refine/SKILL.md) | General | Turn a vague idea into a problem-anchored, implementation-oriented method proposal. Best inserted between `/idea-discovery` and `/auto-review-loop` | Yes |
| 🧪 [`experiment-plan`](skills/experiment-plan/SKILL.md) | General | Turn a refined proposal into a claim-driven experiment roadmap with ablations, budgets, and run order | No |
| 🧭 [`research-refine-pipeline`](skills/research-refine-pipeline/SKILL.md) | General | One-shot chain: `/research-refine` → `/experiment-plan` for method refinement plus experiment planning | Yes |
| 📝 [`grant-proposal`](skills/grant-proposal/SKILL.md) | General | Grant proposal drafting (KAKENHI/NSF/NSFC/ERC/DFG/SNSF/ARC/NWO). Chains `/research-lit` → `/novelty-check` → `/research-review` → `/paper-illustration` | Yes |
| 🎤 [`paper-slides`](skills/paper-slides/SKILL.md) | General | Conference talk slides (beamer → PDF + PPTX) with speaker notes, full talk script + Q&A prep. Auto slide count from talk type | Yes |
| 📐 [`proof-writer`](skills/proof-writer/SKILL.md) | ML Theory | Rigorous theorem/lemma proof drafting — feasibility triage, dependency maps, honest blockage reports | No |
| 📡 [`comm-lit-review`](skills/comm-lit-review/SKILL.md) | Communications / Wireless | Domain-specific literature review — IEEE/ACM/ScienceDirect priority, venue tiering, PHY/MAC/transport/NTN taxonomy | No |
| 🏗️ [`dse-loop`](skills/dse-loop/SKILL.md) | Architecture / EDA | Autonomous design space exploration — iteratively run, analyze, and tune parameters (gem5, Yosys, etc.) | No |
| 🤖 [`idea-discovery-robot`](skills/idea-discovery-robot/SKILL.md) | Robotics / Embodied AI | Workflow 1 adaptation — grounds idea discovery in embodiment, benchmark, sim2real path, and real-robot safety constraints | Yes |
| 🎤 [`paper-slides`](skills/paper-slides/SKILL.md) | General | Conference presentation slides (beamer → PDF + PPTX) with speaker notes, talk script, Q&A prep. 4 talk types | No |

</details>

<details>
<summary><b>🌐 External Projects & Docs (6)</b> — click to expand</summary>

| Name | Domain | Description |
|------|--------|-------------|
| 🛡️ [open-source-hardening-skills](https://github.com/zeyuzhangzyz/open-source-hardening-skills) | DevOps / OSS | 10-skill pipeline to harden research code into production-ready open-source projects — audit, refactor, test, CI, docs, review |
| 📊 [CitationClaw](https://github.com/VisionXLab/CitationClaw) | General | Citation impact analysis — input paper title → citation crawling, scholar identification, tiered analysis, HTML dashboard |
| 🐾 [OpenClaw Adaptation Guide](docs/OPENCLAW_ADAPTATION.md) | General | Use ARIS workflow methodology in [OpenClaw](https://github.com/All-Hands-AI/OpenHands) — skill-to-stage mapping, file-based orchestration, no Claude Code CLI needed |
| 🖱️ [Cursor Adaptation Guide](docs/CURSOR_ADAPTATION.md) | General | Use ARIS skills in [Cursor](https://www.cursor.com/) — `@`-reference skills, MCP setup, workflow mapping, state file recovery across sessions |
| 🎨 [`paper-illustration`](skills/paper-illustration/SKILL.md) | General | AI-generated architecture diagrams via Gemini. Built on [PaperBanana](https://github.com/dwzhu-pku/PaperBanana). Integrated into Workflow 3 |
| 🤖 [`skills-codex`](skills/skills-codex/) | General | Full ARIS skill set mirrored for Codex CLI, including `experiment-bridge`, `grant-proposal`, and `paper-illustration` |
| 🔁 [Codex+Claude Review Bridge](docs/CODEX_CLAUDE_REVIEW_GUIDE.md) | General | Codex executes + Claude reviews via local `claude-review` MCP bridge with async polling |

</details>

## 🔄 Workflows

These skills compose into a full research lifecycle. The four workflows can be used independently or chained together:

- **Exploring a new area (e.g., writing a survey)?** Start with Workflow 1 → `/idea-discovery`
- **Have a plan, need to implement and run?** Workflow 1.5 → `/experiment-bridge`
- **Already have results, need iterative improvement?** Workflow 2 → `/auto-review-loop`
- **Ready to write the paper?** Workflow 3 → `/paper-writing` (or step by step: `/paper-plan` → `/paper-figure` → `/paper-write` → `/paper-compile` → `/auto-paper-improvement-loop`)
- **Full pipeline?** Workflow 1 → 1.5 → 2 → 3 → `/research-pipeline` — from literature survey all the way to submission

> ⚠️ **Important:** These tools accelerate research, but they don't replace your own critical thinking. Always review generated ideas with your domain expertise, question the assumptions, and make the final call yourself. The best research comes from human insight + AI execution, not full autopilot.

### Full Pipeline 🚀

```
/research-lit → /idea-creator → /novelty-check → /research-refine → /experiment-bridge → /auto-review-loop → /paper-plan → /paper-figure → /paper-write → /auto-paper-improvement-loop → submit
  (survey)      (brainstorm)    (verify novel)   (refine method)   (implement+deploy)  (review & fix)      (outline)     (plots)        (LaTeX+PDF)     (review ×2 + format)     (done!)
  ├────────────── Workflow 1: Idea Discovery ──────────────┤ ├ Workflow 1.5 ─┤ ├── Workflow 2 ──┤   ├──────────────── Workflow 3: Paper Writing ──────────────────┤
```

📝 **Blog post:** [梦中科研全流程开源](http://xhslink.com/o/2iV33fYoc7Q)

### Workflow 1: Idea Discovery & Method Refinement 🔍

> **"What's the state of the art? Where are the gaps? How do we solve it?"**

Don't have a concrete idea yet? Just give a research direction — `/idea-discovery` handles the rest:

1. 📚 **Survey** the landscape (recent papers, open problems, recurring limitations)
2. 🧠 **Brainstorm** 8-12 concrete ideas via GPT-5.4 xhigh
3. 🔍 **Filter** by feasibility, compute cost, and quick novelty search
4. 🛡️ **Validate** top ideas with deep novelty check + devil's advocate review
5. 🧪 **Pilot** top 2-3 ideas in parallel on different GPUs (30 min - 2 hr each)
6. 🏆 **Rank** by empirical signal — ideas with positive pilot results rise to the top
7. 🔬 **Refine** the top idea into a problem-anchored proposal via iterative GPT-5.4 review
8. 🧪 **Plan** claim-driven experiments with ablations, budgets, and run order

The output is a ranked `IDEA_REPORT.md` plus a refined proposal (`refine-logs/FINAL_PROPOSAL.md`) and experiment plan (`refine-logs/EXPERIMENT_PLAN.md`) for the top idea. Dead-end ideas are documented too, saving future exploration.

```
┌─────────────────────────────────────────────────────────────────┐
│              Idea Discovery & Method Refinement                  │
│                                                                  │
│   /research-lit    /idea-creator    /novelty-check               │
│   (find papers)    (brainstorm)     (verify novelty)             │
│         │               │                │                       │
│         ▼               ▼                ▼                       │
│   ┌──────────┐    ┌──────────┐     ┌──────────┐                │
│   │ Scan     │───▶│ Generate │────▶│ Check if │                │
│   │ local    │    │ 8-12     │     │ idea is  │                │
│   │ papers + │    │ ideas    │     │ novel    │                │
│   │ search   │    │ + rank   │     │          │                │
│   └──────────┘    └──────────┘     └──────────┘                │
│                         │                │                       │
│                         ▼                ▼                       │
│                   ┌──────────┐     ┌──────────┐                │
│                   │ Filter   │────▶│ External │                │
│                   │ by cost, │     │ LLM      │                │
│                   │ novelty  │     │ evaluates│                │
│                   └──────────┘     └──────────┘                │
│                                          │                       │
│                   /research-refine       ▼                       │
│                   (refine method)   ┌──────────┐                │
│                         │          │ Freeze   │                │
│                         ▼          │ problem  │                │
│                   ┌──────────┐     │ anchor + │                │
│                   │ Iterate  │◀───▶│ refine   │                │
│                   │ until    │     │ method   │                │
│                   │ score≥9  │     └──────────┘                │
│                   └──────────┘          │                       │
│                         │               ▼                       │
│                   /experiment-plan  ┌──────────┐                │
│                         │          │ Claim-   │                │
│                         ▼          │ driven   │                │
│                   ┌──────────┐     │ experiment│               │
│                   │ Plan     │────▶│ roadmap  │                │
│                   │ runs     │     └──────────┘                │
│                   └──────────┘                                  │
│                                                                  │
│   Typical flow:                                                  │
│   1. /research-lit "discrete diffusion models"                   │
│   2. /idea-creator "DLLMs post training"                         │
│   3. Review ranked ideas, pick top 2-3                           │
│   4. /novelty-check "top idea" (deep verification)               │
│   5. /research-review "top idea" (critical feedback)             │
│   6. /research-refine "top idea" (problem anchor + method)       │
│   7. /experiment-plan (claim-driven roadmap)                     │
│   8. /run-experiment → /auto-review-loop                         │
└─────────────────────────────────────────────────────────────────┘
```

**Skills involved:** `research-lit` + `idea-creator` + `novelty-check` + `research-review` + `research-refine-pipeline`

> 💡 **One-command shortcut:** `/idea-discovery "your research direction"` runs this entire workflow automatically.

> 🔄 **Human-in-the-loop:** Each phase presents results and waits for your feedback. Not happy? Tell it what's missing — it refines the prompt and regenerates. Trust the defaults? It auto-proceeds with the top-ranked option. You decide how hands-on to be.

> ⚙️ Pilot experiment budgets (max hours, timeout, GPU budget) are configurable — see [Customization](#%EF%B8%8F-customization).

📝 **Blog post:** [Claude Code 两月 NeurIPS 指北](http://xhslink.com/o/7IvAJQ41IBA)

### Workflow 1.5: Experiment Bridge 🔗

> **"I have a plan. Now implement it, deploy it, and get me initial results."**

Already have an experiment plan (from Workflow 1 or your own)? `/experiment-bridge` turns it into running code:

1. 📋 **Parse** the experiment plan (`refine-logs/EXPERIMENT_PLAN.md`)
2. 💻 **Implement** experiment scripts (reuse existing code, add proper argparse/logging/seeds)
3. 🔍 **GPT-5.4 code review** — cross-model review catches logic bugs before wasting GPU hours (`code review: true` by default)
4. ✅ **Sanity check** — run the smallest experiment first to catch runtime bugs
5. 🚀 **Deploy** full experiment suite to GPU via `/run-experiment`
6. 📊 **Collect** initial results and update the experiment tracker

**Skills involved:** `experiment-bridge` + `run-experiment` + `monitor-experiment`

> 💡 **One-command shortcut:** `/experiment-bridge` reads `refine-logs/EXPERIMENT_PLAN.md` automatically. Or point it to any plan: `/experiment-bridge "my_plan.md"`.

> ⚙️ `AUTO_DEPLOY`, `SANITY_FIRST`, `MAX_PARALLEL_RUNS` are configurable — see [Customization](#%EF%B8%8F-customization).

### Workflow 2: Auto Research Loop 🔁 (sleep & wake up to results)

> **"Review my paper, fix what's wrong, repeat until it's good."**
>
> GPT-5.4 reviews → identifies weaknesses → suggests experiments → Claude Code writes scripts, deploys to GPU, monitors results, rewrites the paper — all while you sleep. Just add your [GPU server config](#%EF%B8%8F-gpu-server-setup-for-auto-experiments) to `CLAUDE.md`.

```
┌─────────────────────────────────────────────────────────────┐
│                    Auto Review Loop                          │
│                                                              │
│   /research-review          /auto-review-loop                │
│   (single deep review)      (autonomous loop)                │
│         │                         │                          │
│         ▼                         ▼                          │
│   ┌──────────┐   ┌──────────┐   ┌──────────┐               │
│   │ External  │──▶│ Implement│──▶│ Monitor  │──▶ repeat     │
│   │ LLM      │   │ fixes    │   │ results  │    until       │
│   │ reviews  │   │ & run    │   │          │    score ≥ 6   │
│   └──────────┘   │ experiments│  └──────────┘               │
│                   └──────────┘                               │
│                                                              │
│   When reviewer suggests a new method direction:             │
│   /novelty-check — verify idea isn't already published       │
│                                                              │
│   Supporting skills:                                         │
│   /run-experiment    — deploy to local/remote GPU            │
│   /analyze-results   — interpret experiment outputs          │
│   /monitor-experiment — check progress, collect results      │
└─────────────────────────────────────────────────────────────┘
```

**Skills involved:** `auto-review-loop` + `research-review` + `novelty-check` + `run-experiment` + `analyze-results` + `monitor-experiment`

> 💡 **One-command shortcut:** `/auto-review-loop "your paper topic"` runs this entire workflow automatically.
>
> **What to pass as argument?** A short topic or scope is enough — the skill automatically reads your project's narrative docs (`NARRATIVE_REPORT.md`), memory files, experiment results, and prior reviews to build the full context for GPT-5.4. Examples:
> - `/auto-review-loop "factorized gap in discrete diffusion LMs"` — broad topic, skill finds everything
> - `/auto-review-loop "focus on Section 3-5, our CRF results are weak"` — targeted scope with hints
> - `/auto-review-loop` — also works: skill reads project files and infers the topic

**🛡️ Key safety features:**

- 🔒 **MAX_ROUNDS = 4** — prevents infinite loops; stops early if score threshold is met
- ⏱️ **> 4 GPU-hour experiments skipped** — won't launch massive jobs; flags them for manual follow-up
- 🧠 **Prefer reframing over new experiments** — when both can address a weakness, chooses the cheaper path
- 🪞 **No hiding weaknesses** — explicit rule: "Do NOT hide weaknesses to game a positive score"
- 🔧 **Fix before re-review** — must actually implement fixes before resubmitting; no empty promises
- 💾 **Compact recovery** — persists state (`REVIEW_STATE.json`) after each round. If the context window fills up and auto-compacts mid-loop, the workflow reads the state file and resumes from where it left off — no human intervention needed

> ⚙️ MAX_ROUNDS, score threshold, and GPU limits are configurable — see [Customization](#%EF%B8%8F-customization).

📝 **Blog post:** [开源 | 睡觉 Claude 自动跑实验改文](http://xhslink.com/o/5cBMTDigNXz)

### Workflow 3: Paper Writing Pipeline 📝

> **"Turn my research narrative into a submission-ready PDF."** Requires a local LaTeX environment — see [Prerequisites](#prerequisites).

```
┌─────────────────────────────────────────────────────────────┐
│                   Paper Writing Pipeline                      │
│                                                               │
│   /paper-plan      /paper-figure     /paper-write             │
│   (outline)        (plots & tables)  (LaTeX draft)            │
│        │                │                 │                   │
│        ▼                ▼                 ▼                   │
│   ┌──────────┐    ┌──────────┐     ┌──────────┐              │
│   │ Claims-  │───▶│ Generate │────▶│ Section  │──┐           │
│   │ Evidence │    │ figures, │     │ by       │  │           │
│   │ Matrix + │    │ tables,  │     │ section  │  │           │
│   │ Section  │    │ LaTeX    │     │ LaTeX    │  │           │
│   │ Plan     │    │ includes │     │ draft    │  │           │
│   └──────────┘    └──────────┘     └──────────┘  │           │
│        │                                          │           │
│        │         /paper-compile                   │           │
│        │         (build PDF)                      │           │
│        │              │                           │           │
│        ▼              ▼                           ▼           │
│   ┌──────────────────────────────────────────────────┐       │
│   │ NARRATIVE_REPORT.md ──► PAPER_PLAN.md ──► paper/ │       │
│   │    (input)             (outline)      (LaTeX+PDF)│       │
│   └──────────────────────────────────────────────────┘       │
│                                                               │
│   Typical flow:                                               │
│   1. Write NARRATIVE_REPORT.md (from Workflow 2 results)      │
│   2. /paper-plan (claims-evidence matrix + section plan)      │
│   3. /paper-figure (comparison tables, training curves, etc.) │
│   4. /paper-write (section-by-section LaTeX generation)       │
│   5. /paper-compile (build PDF, fix errors, page check)       │
│   6. /auto-paper-improvement-loop (review ×2 + format check)  │
└─────────────────────────────────────────────────────────────┘
```

**Skills involved:** `paper-plan` + `paper-figure` + `paper-write` + `paper-compile` + `auto-paper-improvement-loop`

> **One-command shortcut:** `/paper-writing "NARRATIVE_REPORT.md"` runs this entire workflow automatically.

**Input:** A `NARRATIVE_REPORT.md` describing the research: claims, experiments, results, figures. The more detailed the narrative (especially figure descriptions and quantitative results), the better the output. See [`docs/NARRATIVE_REPORT_EXAMPLE.md`](docs/NARRATIVE_REPORT_EXAMPLE.md) for a complete example.

**Output:** A submission-ready `paper/` directory with LaTeX source, clean `.bib` (only cited entries), and compiled PDF.

**Key features:**
- 📐 **Claims-Evidence Matrix** — every claim maps to evidence, every experiment supports a claim
- 📊 **Auto figure generation** — line plots, bar charts, comparison tables from JSON data
- 🧹 **Clean bib** — automated filtering removes uncited entries (948→215 lines in testing). Real BibTeX from [DBLP](https://dblp.org)/[CrossRef](https://www.crossref.org) instead of LLM-generated entries
- 📄 **Flexible sections** — 5-8 sections depending on paper type (theory papers often need 7)
- 🔍 **GPT-5.4 review** — each step optionally reviewed by external LLM
- ✂️ **De-AI polish** — removes AI writing patterns (delve, pivotal, landscape...)
- 🎯 **Page verification** — `pdftotext`-based precise check that main body fits page limit

> ⚠️ **Figure generation scope:** `/paper-figure` auto-generates **data-driven plots** (training curves, bar charts, heatmaps) and **comparison tables** from JSON/CSV. For **architecture diagrams and method figures**, set `illustration: true` to enable `/paper-illustration` (requires `GEMINI_API_KEY`) — it uses a Claude→Gemini→Nano Banana Pro pipeline to generate publication-quality diagrams automatically. Without `illustration: true`, architecture diagrams must be created manually (draw.io, Figma, TikZ).

**Tested end-to-end:** Generated a 9-page ICLR 2026 theory paper (7 sections, 29 citations, 4 figures, 2 comparison tables) from a single NARRATIVE_REPORT.md — zero compilation errors, zero undefined references.

#### Auto Paper Improvement Loop ✨

After Workflow 3 generates the paper, `/auto-paper-improvement-loop` runs 2 rounds of GPT-5.4 xhigh content review → fix → recompile, plus a final format compliance check, autonomously polishing the paper from rough draft to submission-ready.

**Score Progression (Real Test — ICLR 2026 theory paper):**

| Round | Score | Key Changes |
|-------|-------|-------------|
| Round 0 | 4/10 (content) | Baseline |
| Round 1 | 6/10 (content) | Fixed assumptions, softened claims, renamed notation |
| Round 2 | 7/10 (content) | Added synthetic validation, stronger limitations |
| Round 3 | 5→8.5/10 (format) | Removed hero fig, appendix, compressed conclusion, float spacing |

**Final: 8 pages main body (ICLR limit: 9), 0 overfull hbox, ICLR-compliant.** +4.5 points across 3 rounds.

<details>
<summary>Round 1 fixes (6 items)</summary>

1. **CRITICAL — Assumption-model mismatch**: A boundedness assumption contradicted the model's distributional family. Replaced with a tail-compatible assumption and added formal truncation bridge.
2. **CRITICAL — Theory-practice gap**: Theory assumes idealized encoders, experiments use learned nonlinear encoders. Softened "validate" → "demonstrate practical relevance" and added explicit disclaimer.
3. **MAJOR — Missing quantitative metrics**: Added parameter count table (latent vs total) with honest accounting of system cost.
4. **MAJOR — Theorem not self-contained**: Added "Interpretation" paragraph listing all dependencies explicitly.
5. **MAJOR — Overclaim in novelty statement**: Scoped a broad "first convergence guarantee" to precise conditions under which it holds.
6. **MAJOR — Notation confusion**: Renamed a symbol that clashed with another key variable. Added Notation paragraph.

</details>

<details>
<summary>Round 2 fixes (4 items)</summary>

1. **MAJOR — Missing theory-aligned experiments**: Added a synthetic validation subsection directly testing the two main theoretical predictions under controlled conditions.
2. **MAJOR — Overclaim softening**: Replaced strong equivalence claims with appropriately hedged language across all files.
3. **MAJOR — Informal theoretical argument**: Formalized an informal justification into a proper proposition with explicit error bounds.
4. **MINOR — Weak limitations**: Expanded to explicitly list all assumptions and acknowledge missing standard evaluations.

</details>

<details>
<summary>Round 3 format fixes (8 items)</summary>

1. Removed hero figure block (saved ~0.7 pages)
2. Compressed conclusion from 15→9 lines
3. Moved synthetic validation to Appendix A
4. Moved comparison tables to Appendix B
5. Fixed overfull hbox (85pt) with `\resizebox`
6. Added compact float spacing (`\captionsetup`, `\textfloatsep`)
7. Inlined centered question block in introduction
8. Tightened `itemize` environments

</details>

---

## 🧰 All Skills

### 🚀 Full Pipeline

| Skill | Description | Codex MCP? |
|-------|-------------|:---:|
| 🏗️ [`research-pipeline`](skills/research-pipeline/SKILL.md) | **End-to-end**: Workflow 1 → 1.5 → 2 → 3, from research direction to submission | Yes |

### 🔍 Workflow 1: Idea Discovery & Method Refinement

| Skill | Description | Codex MCP? |
|-------|-------------|:---:|
| 🔭 **[`idea-discovery`](skills/idea-discovery/SKILL.md)** | **Pipeline orchestrator** — runs all skills below in sequence | Yes |
| ├ 📚 [`research-lit`](skills/research-lit/SKILL.md) | Multi-source literature search ([Zotero](#-zotero-integration-optional) + [Obsidian](#-obsidian-integration-optional) + local PDFs + [arXiv API](#arxiv-integration) + web) | No |
| ├ 💡 [`idea-creator`](skills/idea-creator/SKILL.md) | Brainstorm 8-12 ideas, filter by feasibility, pilot on GPU, rank by signal | Yes |
| ├ 🔍 [`novelty-check`](skills/novelty-check/SKILL.md) | Verify idea novelty against recent literature (multi-source + GPT-5.4 cross-check) | Yes |
| ├ 🔬 [`research-review`](skills/research-review/SKILL.md) | Single-round deep review from external LLM (xhigh reasoning) | Yes |
| └ 🧭 **[`research-refine-pipeline`](skills/research-refine-pipeline/SKILL.md)** | Refine method + plan experiments in one chain | Yes |
| 　├ 🔬 [`research-refine`](skills/research-refine/SKILL.md) | Problem anchor → iterative method refinement (up to 5 rounds, score ≥ 9) | Yes |
| 　└ 🧪 [`experiment-plan`](skills/experiment-plan/SKILL.md) | Claim-driven experiment roadmap with ablations, budgets, and run order | No |

### 🔗 Workflow 1.5: Experiment Bridge

| Skill | Description | Codex MCP? |
|-------|-------------|:---:|
| 🔗 **[`experiment-bridge`](skills/experiment-bridge/SKILL.md)** | Read experiment plan → implement code → sanity check → deploy to GPU → collect initial results | No |
| ├ 🚀 [`run-experiment`](skills/run-experiment/SKILL.md) | Deploy experiments to local (MPS/CUDA) or remote GPU servers | No |
| └ 👀 [`monitor-experiment`](skills/monitor-experiment/SKILL.md) | Monitor running experiments, check progress, collect results | No |

### 🔁 Workflow 2: Auto Research Loop

| Skill | Description | Codex MCP? |
|-------|-------------|:---:|
| 🔁 **[`auto-review-loop`](skills/auto-review-loop/SKILL.md)** | **Pipeline orchestrator** — autonomous review→fix→re-review (max 4 rounds) | Yes |
| ├ 🔬 [`research-review`](skills/research-review/SKILL.md) | Deep review from external LLM (shared with Workflow 1) | Yes |
| ├ 🔍 [`novelty-check`](skills/novelty-check/SKILL.md) | Verify novelty when reviewer suggests new directions | Yes |
| ├ 🚀 [`run-experiment`](skills/run-experiment/SKILL.md) | Deploy experiments to local (MPS/CUDA) or remote GPU servers | No |
| ├ 📊 [`analyze-results`](skills/analyze-results/SKILL.md) | Analyze experiment results, compute statistics, generate insights | No |
| └ 👀 [`monitor-experiment`](skills/monitor-experiment/SKILL.md) | Monitor running experiments, check progress, collect results | No |
| 🔁 [`auto-review-loop-llm`](skills/auto-review-loop-llm/SKILL.md) | Same as above, but uses any OpenAI-compatible API via [`llm-chat`](mcp-servers/llm-chat/) MCP server | No |

### 📝 Workflow 3: Paper Writing

| Skill | Description | Codex MCP? |
|-------|-------------|:---:|
| 📝 **[`paper-writing`](skills/paper-writing/SKILL.md)** | **Pipeline orchestrator** — runs all skills below in sequence | Yes |
| ├ 📐 [`paper-plan`](skills/paper-plan/SKILL.md) | Claims-evidence matrix, section structure, figure plan, citation scaffolding | Yes |
| ├ 📊 [`paper-figure`](skills/paper-figure/SKILL.md) | Publication-quality matplotlib/seaborn plots + LaTeX comparison tables | Optional |
| ├ 🎨 [`paper-illustration`](skills/paper-illustration/SKILL.md) | AI-generated architecture diagrams and method figures via Gemini (when `illustration: true`) | No (needs Gemini API) |
| ├ ✍️ [`paper-write`](skills/paper-write/SKILL.md) | Section-by-section LaTeX generation (ICLR/NeurIPS/ICML). Anti-hallucination BibTeX via DBLP/CrossRef | Yes |
| ├ 🔨 [`paper-compile`](skills/paper-compile/SKILL.md) | Compile LaTeX to PDF, auto-fix errors, submission readiness checks | No |
| └ 🔄 [`auto-paper-improvement-loop`](skills/auto-paper-improvement-loop/SKILL.md) | 2-round content review + format check (4/10 → 8.5/10) | Yes |

### 🛠️ Standalone / Utility

| Skill | Description | Codex MCP? |
|-------|-------------|:---:|
| 📄 [`arxiv`](skills/arxiv/SKILL.md) | Search, download, and summarize arXiv papers. Standalone or `/research-lit` supplement | No |
| 🎨 [`pixel-art`](skills/pixel-art/SKILL.md) | Generate pixel art SVG illustrations for READMEs, docs, or slides | No |
| 📱 [`feishu-notify`](skills/feishu-notify/SKILL.md) | [Feishu/Lark](#-feishulark-integration-optional) push (webhook) or interactive (bidirectional). Off by default | No |

---

## ⚙️ Setup

### Prerequisites

1. [Claude Code](https://docs.anthropic.com/en/docs/claude-code) installed
2. (For review skills) [Codex CLI](https://github.com/openai/codex) installed and configured as MCP server:
   ```bash
   npm install -g @openai/codex
   claude mcp add codex -s user -- codex mcp-server
   ```
3. (For Workflow 3: paper writing) **LaTeX** environment with `latexmk` and `pdfinfo`:
   ```bash
   # macOS
   brew install --cask mactex    # or: brew install basictex
   brew install poppler          # provides pdfinfo

   # Ubuntu/Debian
   sudo apt install texlive-full latexmk poppler-utils

   # Verify
   latexmk --version && pdfinfo -v
   ```
   > If you only need Workflow 1 & 2 (idea discovery + auto review), LaTeX is not required.

### Install Skills

```bash
git clone https://github.com/wanshuiyin/Auto-claude-code-research-in-sleep.git
cd Auto-claude-code-research-in-sleep

# Install all skills globally
cp -r skills/* ~/.claude/skills/

# Or install specific skills
cp -r skills/auto-review-loop ~/.claude/skills/
cp -r skills/research-lit ~/.claude/skills/
```

### Usage

```
# Workflow 1: Idea Discovery
> /idea-discovery "your research direction"          # full pipeline
> /research-lit "topic"                              # just literature survey (all sources)
> /research-lit "topic" — sources: zotero, web        # mix and match sources
> /research-lit "topic" — arxiv download: true         # also download top arXiv PDFs
> /arxiv "discrete diffusion" — download               # standalone arXiv search + download
> /idea-creator "topic"                              # just brainstorm

# Workflow 2: Auto Research Loop
> /auto-review-loop "your paper topic"               # review → fix → repeat
> /research-review "your paper"                      # single deep review

# Workflow 3: Paper Writing
> /paper-writing "NARRATIVE_REPORT.md"               # full pipeline
> /paper-plan "NARRATIVE_REPORT.md"                  # just outline
> /paper-compile "paper/"                            # just compile

# Full Pipeline
> /research-pipeline "your research direction"       # Workflow 1 → 2 → 3 end-to-end

# Supporting Skills
> /run-experiment train.py --lr 1e-4 --epochs 100
> /analyze-results figures/*.json
> /monitor-experiment server5
```

### 🌙 Auto-Allow for Overnight Runs (Optional)

To run the auto-review loop without clicking permission prompts, add to `.claude/settings.local.json`:

```json
{
  "permissions": {
    "allow": [
      "mcp__codex__codex",
      "mcp__codex__codex-reply",
      "Write",
      "Edit",
      "Skill(auto-review-loop)"
    ]
  }
}
```

<details>
<summary><h3>🖥️ GPU Server Setup (For Auto-Experiments)</h3></summary>

When GPT-5.4 says "run an ablation study" or "add a baseline comparison", Claude Code automatically writes the experiment script and deploys it to your GPU server. For this to work, Claude Code needs to know your server environment.

Add your server info to your project's `CLAUDE.md`:

```markdown
## Remote Server

- SSH: `ssh my-gpu-server` (key-based auth, no password)
- GPU: 4x A100
- Conda env: `research` (Python 3.10 + PyTorch)
- Activate: `eval "$(/opt/conda/bin/conda shell.bash hook)" && conda activate research`
- Code directory: `/home/user/experiments/`
- Use `screen` for background jobs: `screen -dmS exp0 bash -c '...'`
```

Claude Code reads this and knows how to SSH in, activate the environment, and launch experiments. GPT-5.4 (the reviewer) only decides **what** experiments to run — Claude Code figures out **how** based on your `CLAUDE.md`.

If you are already on the GPU server, you can add the following to your `CLAUDE.md`:
```markdown
## GPU Environment

- This machine has direct GPU access (no SSH needed)
- GPU: 4x A100 80GB
- Experiment environment: `YOUR_CONDA_ENV` (Python 3.x + PyTorch)
- Activate before any Python command: `The command to activate your experiment environment` (uv, conda, etc.)
- Code directory: `/home/YOUR_USERNAME/YOUR_CODE_DIRECTORY/`
```

**No server?** The review and rewriting skills still work without GPU access. Only experiment-related fixes will be skipped (flagged for manual follow-up).

</details>

<details>
<summary><b>📚 Zotero Integration (Optional)</b></summary>

If you use [Zotero](https://www.zotero.org/) to manage your paper library, `/research-lit` can search your collections, read your annotations/highlights, and export BibTeX — all before searching the web.

**Recommended: [zotero-mcp](https://github.com/54yyyu/zotero-mcp)** (1.8k⭐, semantic search, PDF annotations, BibTeX export)

```bash
# Install
uv tool install zotero-mcp-server   # or: pip install zotero-mcp-server

# Add to Claude Code (Local API — requires Zotero desktop running)
claude mcp add zotero -s user -- zotero-mcp -e ZOTERO_LOCAL=true

# Or use Web API (works without Zotero running)
claude mcp add zotero -s user -- zotero-mcp \
  -e ZOTERO_API_KEY=your_key -e ZOTERO_USER_ID=your_id
```

> Get your API key at https://www.zotero.org/settings/keys

**What it enables in `/research-lit`:**
- 🔍 Search your Zotero library by topic (including semantic/vector search)
- 📂 Browse collections and tags
- 📝 Read your PDF annotations and highlights (what you personally found important)
- 📄 Export BibTeX for direct use in paper writing

**Not using Zotero?** No problem — `/research-lit` automatically skips Zotero and uses local PDFs + web search instead.

</details>

<details>
<summary><b>📓 Obsidian Integration (Optional)</b></summary>

If you use [Obsidian](https://obsidian.md/) for research notes, `/research-lit` can search your vault for paper summaries, tagged references, and your own insights.

**Recommended: [mcpvault](https://github.com/bitbonsai/mcpvault)** (760⭐, no Obsidian app needed, 14 tools, BM25 search)

```bash
# Add to Claude Code (point to your vault path)
claude mcp add obsidian-vault -s user -- npx @bitbonsai/mcpvault@latest /path/to/your/vault
```

**Optional complement: [obsidian-skills](https://github.com/kepano/obsidian-skills)** (13.6k⭐, by Obsidian CEO) — teaches Claude to understand Obsidian-specific Markdown (wikilinks, callouts, properties). Copy to your vault:

```bash
git clone https://github.com/kepano/obsidian-skills.git
cp -r obsidian-skills/.claude /path/to/your/vault/
```

**What it enables in `/research-lit`:**
- 🔍 Search your vault for notes on the research topic
- 🏷️ Find notes by tags (e.g., `#paper-review`, `#diffusion-models`)
- 📝 Read your processed summaries and insights (more valuable than raw papers)
- 🔗 Follow wikilinks to discover related notes

**Not using Obsidian?** No problem — `/research-lit` automatically skips Obsidian and works as before.

> 💡 **Zotero + Obsidian together**: Many researchers use Zotero for paper storage and Obsidian for notes. Both integrations work simultaneously — `/research-lit` checks Zotero first (raw papers + annotations), then Obsidian (your processed notes), then local PDFs, then web search.

#### arXiv Integration

`/research-lit` automatically queries the arXiv API for structured metadata (title, abstract, full author list, categories) — richer than web search snippets. No setup required.

By default, only metadata is fetched (no files downloaded). To also download the most relevant PDFs:

```
/research-lit "topic" — arxiv download: true              # download top 5 PDFs
/research-lit "topic" — arxiv download: true, max download: 10  # download up to 10
```

For standalone arXiv access, use the dedicated [`/arxiv`](skills/arxiv/SKILL.md) skill:

```
/arxiv "attention mechanism"           # search
/arxiv "2301.07041" — download         # download specific paper
```

</details>

<details>
<summary><h3>📱 Feishu/Lark Integration (Optional)</h3></summary>

Get mobile notifications when experiments finish, reviews score, or checkpoints need your input — without sitting in front of the terminal.

| Push Only (group cards) | Interactive (private chat) |
|:-:|:-:|
| <img src="assets/feishu_push.png" width="450" /> | <img src="assets/feishu_interactive.jpg" width="450" /> |

**Three modes — you choose per-project:**

| Mode | What happens | You need |
|------|-------------|----------|
| **Off** (default) | Nothing. Pure CLI, no Feishu | Nothing |
| **Push only** | Webhook notifications at key events. Mobile push, no reply | Feishu bot webhook URL |
| **Interactive** | Full bidirectional. Approve/reject ideas, reply to checkpoints from Feishu | [feishu-claude-code](https://github.com/joewongjc/feishu-claude-code) running |

<details>
<summary><b>Push Only Setup (5 min)</b></summary>

Group notifications with rich cards — experiment done, review scored, pipeline complete. Mobile push, no reply needed.

**Step 1: Create a Feishu group bot**

1. Open your Feishu group (or create a test group)
2. Group Settings → Bots → Add Bot → **Custom Bot**
3. Name it (e.g., `ARIS Notifications`), copy the **Webhook URL**
4. Security: add custom keyword `ARIS` (all notifications include this word), or leave unrestricted

**Step 2: Create config file**

```bash
cat > ~/.claude/feishu.json << 'EOF'
{
  "mode": "push",
  "webhook_url": "https://open.feishu.cn/open-apis/bot/v2/hook/YOUR_WEBHOOK_ID"
}
EOF
```

**Step 3: Test it**

```bash
curl -s -X POST "YOUR_WEBHOOK_URL" \
  -H "Content-Type: application/json" \
  -d '{
    "msg_type": "interactive",
    "card": {
      "header": {"title": {"tag": "plain_text", "content": "🧪 ARIS Test"}, "template": "blue"},
      "elements": [{"tag": "markdown", "content": "Push mode working! 🎉"}]
    }
  }'
```

You should see a blue card in your group. Skills will now automatically send rich cards at key events:

| Event | Card color | Content |
|-------|-----------|---------|
| Review scored ≥ 6 | 🟢 Green | Score, verdict, top weaknesses |
| Review scored < 6 | 🟠 Orange | Score, verdict, action items |
| Experiment complete | 🟢 Green | Results table, delta vs baseline |
| Checkpoint waiting | 🟡 Yellow | Question, options, context |
| Error | 🔴 Red | Error message, suggested fix |
| Pipeline done | 🟣 Purple | Score progression, deliverables |

</details>

<details>
<summary><b>Interactive Setup (15 min)</b></summary>

Everything Push mode does, **plus** bidirectional private chat with Claude Code via Feishu. Approve/reject ideas, reply to checkpoints, give custom instructions — all from your phone.

**How it works**: Push cards go to the **group** (everyone sees status). Interactive conversations happen in **private chat** with the bot (you reply, Claude Code acts on it).

**Step 1: Complete Push setup above first** (you'll keep both)

**Step 2: Create a Feishu app on [open.feishu.cn](https://open.feishu.cn/app)**

1. Click **Create Enterprise App** → name it (e.g., `ARIS Claude Bot`) → create
2. Left menu → **Add Capabilities** → check **Bot**
3. Left menu → **Permissions** → search and enable these 5 permissions:

| Permission | Scope | Why |
|-----------|-------|-----|
| `im:message` | Send & receive messages | Core messaging |
| `im:message:send_as_bot` | Send as bot | Bot replies |
| `im:message.group_at_msg:readonly` | Receive group @mentions | Group messages |
| `im:message.p2p_msg:readonly` | **Receive private messages** | ⚠️ **Easy to miss!** Without this, the bot connects but never receives your messages |
| `im:resource` | Access attachments | Images/files |

4. Left menu → **Events & Callbacks** → select **Long Connection** mode → add event: `im.message.receive_v1` → save

> ⚠️ **Important**: The "Long Connection" page may show "未检测到应用连接信息" — this is normal. You need to start the bridge first (Step 3), then come back and save.

5. Left menu → **Version Management** → **Create Version** → fill description → **Submit for Review**

> For personal/test Feishu organizations, approval is usually instant.

**Step 3: Deploy the bridge**

```bash
git clone https://github.com/joewongjc/feishu-claude-code.git
cd feishu-claude-code
python3 -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt

# Configure
cp .env.example .env
```

Edit `.env`:

```bash
FEISHU_APP_ID=cli_your_app_id          # From app credentials page
FEISHU_APP_SECRET=your_app_secret      # From app credentials page
DEFAULT_MODEL=claude-opus-4-6          # ⚠️ Default is sonnet — change to opus for best results
DEFAULT_CWD=/path/to/your/project      # Working directory for Claude Code
PERMISSION_MODE=bypassPermissions      # Or "default" for safer mode
```

> ⚠️ **Model matters**: The default `claude-sonnet-4-6` works but may struggle with complex project context. `claude-opus-4-6` correctly identified 18 ARIS skills on first try where sonnet could not.

Start the bridge:

```bash
python main.py
# Expected output:
# ✅ 连接飞书 WebSocket 长连接（自动重连）...
# [Lark] connected to wss://msg-frontier.feishu.cn/ws/v2?...
```

For long-running use, put it in a screen session:

```bash
screen -dmS feishu-bridge bash -c 'cd /path/to/feishu-claude-code && source .venv/bin/activate && python main.py'
```

**Step 4: Save event config** — Go back to Feishu Open Platform → Events & Callbacks → the long connection should now show "已检测到连接" → **Save**

> If you published the app version before the bridge was running, you may need to create a new version (e.g., 1.0.1) and re-publish after saving event config.

**Step 5: Test private chat**

1. In Feishu, find the bot in your contacts (search by app name)
2. Send it a message: `你好`
3. It should reply via Claude Code

**If the bot doesn't reply**: Send `/new` to reset the session, then try again. Common issues:

| Symptom | Cause | Fix |
|---------|-------|-----|
| Bot connects but never receives messages | Missing `im:message.p2p_msg:readonly` permission | Add permission → create new version → publish |
| Bot replies but doesn't know your project | `DEFAULT_CWD` points to wrong directory | Edit `.env` → restart bridge |
| Bot replies but seems less capable | Using `claude-sonnet-4-6` | Change to `claude-opus-4-6` in `.env` → restart |
| Old session has stale context | Session cached from before config change | Send `/new` in chat to start fresh session |
| "未检测到应用连接信息" when saving events | Bridge not running yet | Start bridge first, then save event config |

**Step 6: Update ARIS config**

```bash
cat > ~/.claude/feishu.json << 'EOF'
{
  "mode": "interactive",
  "webhook_url": "https://open.feishu.cn/open-apis/bot/v2/hook/YOUR_WEBHOOK_ID",
  "interactive": {
    "bridge_url": "http://localhost:5000",
    "timeout_seconds": 300
  }
}
EOF
```

Now skills will:
- **Push** rich cards to the group (status notifications, everyone sees)
- **Private chat** you for decisions (checkpoints, approve/reject, custom instructions)

#### Which skills send notifications?

| Skill | Events | Push | Interactive |
|-------|--------|------|-------------|
| `/auto-review-loop` | Review scored (each round), loop complete | Score + verdict | + wait for continue/stop |
| `/auto-paper-improvement-loop` | Review scored, all rounds done | Score progression | Score progression |
| `/run-experiment` | Experiments deployed | GPU assignment + ETA | GPU assignment + ETA |
| `/monitor-experiment` | Results collected | Results table | Results table |
| `/idea-discovery` | Phase transitions, final report | Summary at each phase | + approve/reject at checkpoints |
| `/research-pipeline` | Stage transitions, pipeline done | Stage summary | + approve/reject |

</details>

**Not using Feishu?** No problem — without `~/.claude/feishu.json`, all skills behave exactly as before. Zero overhead, zero side effects.

> 💡 **Alternative IM platforms**: The push-only webhook pattern works with any service that accepts incoming webhooks (Slack, Discord, DingTalk, WeChat Work). Just change the `webhook_url` and card format in `feishu-notify/SKILL.md`. For bidirectional support, see [cc-connect](https://github.com/chenhg5/cc-connect) (multi-platform bridge) or [clawdbot-feishu](https://github.com/m1heng/clawdbot-feishu).

</details>

## 🎛️ Customization

Skills are plain Markdown files. Fork and customize:

> 💡 **Parameter pass-through**: Parameters flow down the call chain automatically. For example, `/research-pipeline "topic" — sources: zotero, arxiv download: true` passes `sources` and `arxiv download` through `idea-discovery` all the way down to `research-lit`. You can set any downstream parameter at any level — just add `— key: value` to your command.
>
> ```
> research-pipeline  ──→  idea-discovery      ──→  research-lit
>                    ──→  experiment-bridge    ──→  run-experiment
>                    ──→  auto-review-loop
>                                             ──→  idea-creator
>                                             ──→  novelty-check
>                                             ──→  research-review
> ```

### Full Research Pipeline (`research-pipeline`)

| Constant | Default | Description | Pass-through |
|----------|---------|-------------|:---:|
| `AUTO_PROCEED` | true | Auto-continue with top-ranked option if user doesn't respond | → `idea-discovery` |
| `ARXIV_DOWNLOAD` | false | Download top arXiv PDFs after literature search | → `idea-discovery` → `research-lit` |
| `HUMAN_CHECKPOINT` | false | When `true`, pause after each review round for approval | → `auto-review-loop` |
| `WANDB` | false | Auto-add W&B logging to experiments | → `experiment-bridge` → `run-experiment` |
| `CODE_REVIEW` | true | GPT-5.4 reviews experiment code before deployment | → `experiment-bridge` |
| `ILLUSTRATION` | false | Auto-generate method diagrams via Gemini. Requires `GEMINI_API_KEY` | → `paper-writing` → `paper-illustration` |

Override inline: `/research-pipeline "topic" — auto proceed: false, wandb: true, illustration: true`

### Auto Review Loop (`auto-review-loop`)

| Constant | Default | Description |
|----------|---------|-------------|
| `MAX_ROUNDS` | 4 | Maximum review→fix→re-review iterations |
| `POSITIVE_THRESHOLD` | 6/10 | Score at which the loop stops (submission-ready) |
| `> 4 GPU-hour skip` | 4h | Experiments exceeding this are flagged for manual follow-up |

### Idea Discovery (`idea-discovery` / `idea-creator`)

| Constant | Default | Description | Pass-through |
|----------|---------|-------------|:---:|
| `PILOT_MAX_HOURS` | 2h | Skip any pilot estimated to take longer per GPU | — |
| `PILOT_TIMEOUT_HOURS` | 3h | Hard timeout — kill runaway pilots, collect partial results | — |
| `MAX_PILOT_IDEAS` | 3 | Maximum number of ideas to pilot in parallel | — |
| `MAX_TOTAL_GPU_HOURS` | 8h | Total GPU budget across all pilots | — |
| `AUTO_PROCEED` | true | Auto-continue with top-ranked option if user doesn't respond | — |
| `ARXIV_DOWNLOAD` | false | Download top arXiv PDFs after literature search | → `research-lit` |

Override inline: `/idea-discovery "topic" — pilot budget: 4h per idea, sources: zotero, arxiv download: true`

### Experiment Bridge (`experiment-bridge`)

| Constant | Default | Description |
|----------|---------|-------------|
| `CODE_REVIEW` | true | GPT-5.4 xhigh reviews code before deployment. Catches logic bugs before wasting GPU hours |
| `AUTO_DEPLOY` | true | Automatically deploy experiments after implementation + review. Set `false` to manually inspect |
| `SANITY_FIRST` | true | Run smallest experiment first to catch setup bugs before full deployment |
| `MAX_PARALLEL_RUNS` | 4 | Maximum experiments to deploy in parallel (limited by available GPUs) |
| `WANDB` | false | Auto-add W&B logging. Requires `wandb_project` in CLAUDE.md |

Override inline: `/experiment-bridge — code review: false, wandb: true`

### Literature Search (`research-lit`)

| Constant | Default | Description |
|----------|---------|-------------|
| `PAPER_LIBRARY` | `papers/`, `literature/` | Local directories to scan for PDFs before searching online |
| `MAX_LOCAL_PAPERS` | 20 | Max local PDFs to scan (first 3 pages each) |
| `SOURCES` | `all` | Which sources to search: `zotero`, `obsidian`, `local`, `web`, or `all` (comma-separated) |
| `ARXIV_DOWNLOAD` | false | When `true`, download top relevant arXiv PDFs to PAPER_LIBRARY after search |
| `ARXIV_MAX_DOWNLOAD` | 5 | Maximum number of PDFs to download when `ARXIV_DOWNLOAD = true` |

Override inline: `/research-lit "topic" — sources: zotero, web`, `/research-lit "topic" — arxiv download: true, max download: 10`

### Paper Writing (`paper-write`)

| Constant | Default | Description |
|----------|---------|-------------|
| `DBLP_BIBTEX` | true | Fetch real BibTeX from DBLP/CrossRef instead of LLM-generated entries |
| `TARGET_VENUE` | `ICLR` | Target venue format: `ICLR`, `NeurIPS`, `ICML` |
| `ANONYMOUS` | true | Use anonymous author block for blind review |
| `MAX_PAGES` | 9 | Main body page limit (excluding references) |
| `ILLUSTRATION` | false | Auto-generate architecture/method diagrams via Gemini. Requires `GEMINI_API_KEY` |

Override inline: `/paper-write — target venue: NeurIPS, illustration: true`

### General (all skills using Codex MCP)

| Constant | Default | Description |
|----------|---------|-------------|
| `REVIEWER_MODEL` | `gpt-5.4` | OpenAI model used via Codex MCP. Also available: `gpt-5.3-codex`, `gpt-5.2-codex`, `o3`. See [supported models](https://developers.openai.com/codex/models/) for full list. |

- **Prompt templates** — tailor the review persona and evaluation criteria
- **`allowed-tools`** — restrict or expand what each skill can do

## 🔀 Alternative Model Combinations

Don't have Claude / OpenAI API access? You can swap in other models — same cross-model architecture, different providers.

> ⭐ **We strongly recommend Claude + GPT-5.4 (default setup).** It's the most tested and reliable combination. Alternative setups work but may require prompt tuning.

| | Executor | Reviewer | Need Claude API? | Need OpenAI API? | Guide |
|---|----------|----------|:---:|:---:|-------|
| **Default** ⭐ | Claude Opus/Sonnet | GPT-5.4 (Codex MCP) | Yes | Yes | [Quick Start](#-quick-start) |
| **Alt A** | GLM-5 (Z.ai) | GPT-5.4 (Codex MCP) | No | Yes | [Setup below](#alt-a-glm--gpt) |
| **Alt B** | GLM-5 (Z.ai) | MiniMax-M2.5 | No | No | [MINIMAX_MCP_GUIDE](docs/MINIMAX_MCP_GUIDE.md) |
| **Alt C** | Any CC-compatible | Any OpenAI-compatible | No | No | [LLM_API_MIX_MATCH_GUIDE](docs/LLM_API_MIX_MATCH_GUIDE.md) |
| **Alt D** | Kimi-K2.5 / Qwen3.5+ | GLM-5 / MiniMax-M2.5 | No | No | [ALI_CODING_PLAN_GUIDE](docs/ALI_CODING_PLAN_GUIDE.md) |
| **Alt E** 🆓 | DeepSeek-V3.1 / Qwen3-Coder | DeepSeek-R1 / Qwen3-235B | No | No | [MODELSCOPE_GUIDE](docs/MODELSCOPE_GUIDE.md) |
| **Alt F** | Codex CLI (GPT-5.4) | Codex `spawn_agent` (GPT-5.4) | No | Yes | [skills-codex/](skills/skills-codex/) |
| **Alt G** 🆕 | Codex CLI | Claude Code CLI (`claude-review` MCP) | No* | No* | [CODEX_CLAUDE_REVIEW_GUIDE](docs/CODEX_CLAUDE_REVIEW_GUIDE.md) |

**Alt C** supports tested providers: GLM (Z.ai), Kimi (Moonshot), LongCat (Meituan) as executors; DeepSeek, MiniMax as reviewers. Any OpenAI-compatible API should also work via the generic [`llm-chat`](mcp-servers/llm-chat/) MCP server. **Alt D** uses [Alibaba Coding Plan](https://bailian.console.aliyun.com/) — one API key for both executor and reviewer, 4 models included (Kimi, Qwen, GLM, MiniMax). **Alt E** uses [ModelScope](https://www.modelscope.cn/) — **free** (2000 calls/day), one key, no automation restrictions. **Alt G** keeps Codex as executor but swaps the reviewer to Claude Code CLI via the local `claude-review` MCP bridge, with async polling for long paper/review prompts.

\* Alt G normally relies on local Codex CLI and Claude Code CLI logins. Direct API keys are optional, not required.

### Alt A: GLM + GPT

Only replace the executor (Claude → GLM), keep GPT-5.4 as reviewer via Codex MCP.

```bash
npm install -g @anthropic-ai/claude-code
npm install -g @openai/codex
codex setup   # set model to gpt-5.4
```

Configure `~/.claude/settings.json`:

```json
{
    "env": {
        "ANTHROPIC_AUTH_TOKEN": "your_zai_api_key",
        "ANTHROPIC_BASE_URL": "https://api.z.ai/api/anthropic",
        "API_TIMEOUT_MS": "3000000",
        "ANTHROPIC_DEFAULT_HAIKU_MODEL": "glm-4.5-air",
        "ANTHROPIC_DEFAULT_SONNET_MODEL": "glm-4.7",
        "ANTHROPIC_DEFAULT_OPUS_MODEL": "glm-5"
    },
    "mcpServers": {
        "codex": {
            "command": "/opt/homebrew/bin/codex",
            "args": ["mcp-server"]
        }
    }
}
```

Codex CLI uses your existing `OPENAI_API_KEY` (from `~/.codex/config.toml` or environment) — no extra config needed for the reviewer side.

### Alt B: GLM + MiniMax

No Claude or OpenAI API needed. Uses a custom MiniMax MCP server instead of Codex (because MiniMax doesn't support OpenAI's Responses API). Full guide: [`docs/MINIMAX_MCP_GUIDE.md`](docs/MINIMAX_MCP_GUIDE.md).

### Alt C: Any Executor + Any Reviewer

Mix and match freely using the generic `llm-chat` MCP server. Supports any OpenAI-compatible API as reviewer. Full guide: [`docs/LLM_API_MIX_MATCH_GUIDE.md`](docs/LLM_API_MIX_MATCH_GUIDE.md).

Example combinations: GLM + DeepSeek, Kimi + MiniMax, Claude + DeepSeek, LongCat + GLM, etc.

### After Setup: Install Skills & Verify

```bash
git clone https://github.com/wanshuiyin/Auto-claude-code-research-in-sleep.git
cd Auto-claude-code-research-in-sleep
cp -r skills/* ~/.claude/skills/
claude
```

> **⚠️ For non-Claude executors (GLM, Kimi, etc.):** Let the model read through the project once to ensure skills are correctly parsed. This is especially important if you've [rewritten skills](#-alternative-model-combinations) to use a different reviewer MCP (e.g., `mcp__llm-chat__chat` instead of `mcp__codex__codex`) — the new executor needs to understand the changed tool call patterns:
>
> ```
> Read through this project and verify all skills are working:
> /idea-creator, /research-review, /auto-review-loop, /novelty-check,
> /idea-discovery, /research-pipeline, /research-lit, /run-experiment,
> /analyze-results, /monitor-experiment, /pixel-art
> ```

> ⚠️ **Note:** Alternative models may behave differently from Claude and GPT-5.4. You may need to tune prompt templates for best results. The core cross-model architecture remains the same.

## 📋 Roadmap

### Done

- [x] **Human-in-the-loop checkpoints** — idea-discovery and research-pipeline pause at key decision points for user approval. Configurable via `AUTO_PROCEED` (default: auto-continue; set `false` to always wait)
- [x] **Alternative model combinations** — [GLM + GPT, GLM + MiniMax](#-alternative-model-combinations) fully documented with setup guides. No Claude or OpenAI API required
- [x] **Workflow 3: Paper Writing Pipeline** — full chain: `/paper-plan` → `/paper-figure` → `/paper-write` → `/paper-compile`. ICLR/NeurIPS/ICML templates, claims-evidence matrix, publication-quality figures, latexmk auto-fix. Inspired by [claude-scholar](https://github.com/Galaxy-Dawn/claude-scholar), [Research-Paper-Writing-Skills](https://github.com/Master-cai/Research-Paper-Writing-Skills), [baoyu-skills](https://github.com/jimliu/baoyu-skills)

<details>
<summary>Show 6 more completed items</summary>

- [x] **Configurable REVIEWER_MODEL** — all Codex-dependent skills support custom reviewer model (default `gpt-5.4`, also works with `gpt-5.3-codex`, `gpt-5.2-codex`, `o3`, etc.)
- [x] **Local paper library scanning** — `/research-lit` scans local `papers/` and `literature/` directories before external search, leveraging papers you've already read
- [x] **Idea Discovery pipeline** — `/idea-discovery` orchestrates research-lit → idea-creator → novelty-check → research-review in one command, with pilot experiments on GPU
- [x] **Full research pipeline** — `/research-pipeline` chains Workflow 1 (idea discovery) → implementation → Workflow 2 (auto-review-loop) end-to-end
- [x] **Peer review skill** — `/peer-review` for reviewing others' papers as a conference reviewer, with GPT-5.4 meta-review (planned; currently use `/research-review` with a paper PDF)
- [x] **Cross-model collaboration** — Claude Code (executor) × Codex GPT-5.4 xhigh (reviewer) architecture, avoiding single-model self-play local minima
- [x] **Feishu/Lark integration** — three modes (off/push/interactive), configurable via `~/.claude/feishu.json`. Push-only needs just a webhook URL; interactive uses [feishu-claude-code](https://github.com/joewongjc/feishu-claude-code). Off by default — zero impact on existing workflows. See [setup guide](#-feishulark-integration-optional)
- [x] **Zotero MCP integration** — `/research-lit` searches Zotero collections, reads annotations/highlights, exports BibTeX. Recommended: [zotero-mcp](https://github.com/54yyyu/zotero-mcp) (1.8k⭐). See [setup guide](#-zotero-integration-optional)
- [x] **Obsidian integration** — `/research-lit` searches Obsidian vault for research notes, tagged references, wikilinks. Recommended: [mcpvault](https://github.com/bitbonsai/mcpvault) (760⭐) + [obsidian-skills](https://github.com/kepano/obsidian-skills) (13.6k⭐). See [setup guide](#-obsidian-integration-optional)
- [x] **More executor × reviewer combinations** — any OpenAI-compatible API works via [`llm-chat`](mcp-servers/llm-chat/) MCP server. GLM, MiniMax, Kimi, LongCat, DeepSeek all tested — no Claude or OpenAI API required

</details>

### Planned

- [x] **GitHub-based code sync** — `/run-experiment` now supports `code_sync: git` in CLAUDE.md: `git push` locally → `ssh server "git pull"` on the server. Default remains `rsync` (zero breaking changes)
- [x] **W&B integration** — `/run-experiment` auto-adds `wandb.init()` + `wandb.log()` to training scripts when `wandb: true` in CLAUDE.md. `/monitor-experiment` pulls training curves from W&B for richer diagnostics. Default off, zero impact when unconfigured
- [ ] **Daemon mode** — auto-restart Claude Code session via `launchd`/`systemd` for true unattended operation. Currently the orchestration layer requires an active CLI session; state files (`REVIEW_STATE.json`, `AUTO_REVIEW.md`) support resuming across sessions, but relaunch is manual ([#11](https://github.com/wanshuiyin/Auto-claude-code-research-in-sleep/issues/11))
- [ ] **Reference-style figure generation** — read figures from reference PDFs → identify chart type, color scheme, layout → generate same-style figures with your own data. Two sub-goals:
  - **Data charts** (medium): extract color/font style from reference → apply as matplotlib rcParams → generate same-style plots with your data
  - ~~**Method diagrams** (hard)~~: ✅ Solved by [`paper-illustration`](skills/paper-illustration/SKILL.md) — Claude→Gemini→Nano Banana Pro pipeline, integrated into Workflow 3
  - Building blocks: [PaperBanana](https://github.com/dwzhu-pku/PaperBanana) (5-agent framework: Retriever→Stylist→Visualizer→Critic, NeurIPS benchmark), [Nano Banana Pro](https://deepmind.google/models/gemini-image/pro/) (Gemini image model), [FigureLabs](https://www.figurelabs.ai/) (commercial, PDF import). An [OpenClaw version](https://clawhub.ai/skills/paperbanana) exists on ClawHub but needs rewrite for Claude Code (flagged as incomplete + security concerns)
  - Would enhance `/paper-figure` to accept `— reference: paper.pdf` for style matching
- [x] **ModelScope integration** — [ModelScope guide](docs/MODELSCOPE_GUIDE.md) added as Alt E. Free (2000 calls/day), one API key, dual-protocol (Anthropic + OpenAI), no automation restrictions. DeepSeek-V3.1 + DeepSeek-R1 recommended

## 💬 Community

**Domain-specific skills welcome!** The core skills cover general research workflows, but every field has its own tools and patterns. We welcome PRs that add new skills for your domain — EDA, bioinformatics, robotics, HPC, or anything else. Just add a `skills/your-skill/SKILL.md` and open a PR. See [`dse-loop`](skills/dse-loop/SKILL.md) for an example.

Join the WeChat group for discussion on Claude Code + AI-driven research workflows:

<img src="docs/wechat_group.jpg" alt="WeChat Group QR Code" width="300">

## 📖 Citation

If you use ARIS in your research, please cite:

```bibtex
@misc{yang2026aris,
    author       = {Yang, Ruofeng and Li, Yongcan and Li, Shuai},
    title        = {ARIS: Fully Autonomous Research via Adversarial Multi-Agent Collaboration},
    year         = {2026},
    organization = {GitHub},
    url          = {https://github.com/wanshuiyin/Auto-claude-code-research-in-sleep},
}
```

## ⭐ Star History

![GitHub stars](https://img.shields.io/github/stars/wanshuiyin/Auto-claude-code-research-in-sleep?style=social)

[![Star History Chart](https://api.star-history.com/svg?repos=wanshuiyin/Auto-claude-code-research-in-sleep&type=Date&v=20260318)](https://star-history.com/#wanshuiyin/Auto-claude-code-research-in-sleep&Date)

## 🙏 Acknowledgements

ARIS is inspired by:

- 🧪 [AI Scientist](https://github.com/SakanaAI/AI-Scientist) (Sakana AI) — Automated research pioneer
- 📖 [AutoResearch](https://github.com/karpathy/autoresearch) (Andrej Karpathy) — End-to-end research automation
- 🔭 [FARS](https://analemma.ai/blog/introducing-fars/) (Analemma) — Fully Automated Research System
- 🎨 [PaperBanana](https://github.com/dwzhu-pku/PaperBanana) (PKU) — Multi-agent academic illustration framework

This project builds on and integrates with many excellent open-source projects:

**Core Infrastructure**
- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) — Anthropic's CLI for Claude, the execution backbone
- [Codex CLI](https://github.com/openai/codex) — OpenAI's CLI, used as MCP server for cross-model review

**Zotero Integration** ([setup guide](#-zotero-integration-optional))
- [zotero-mcp](https://github.com/54yyyu/zotero-mcp) — Zotero MCP server with semantic search and PDF annotations
- [Zotero](https://www.zotero.org/) — Open-source reference manager

**Obsidian Integration** ([setup guide](#-obsidian-integration-optional))
- [mcpvault](https://github.com/bitbonsai/mcpvault) — Obsidian vault MCP server (no app required)
- [obsidian-skills](https://github.com/kepano/obsidian-skills) — Claude Code skills for Obsidian Markdown by Steph Ango (Obsidian CEO)

**Paper Writing Inspiration**
- [claude-scholar](https://github.com/Galaxy-Dawn/claude-scholar) — Academic paper writing with Claude
- [Research-Paper-Writing-Skills](https://github.com/Master-cai/Research-Paper-Writing-Skills) — Paper writing skill templates
- [baoyu-skills](https://github.com/jimliu/baoyu-skills) — Claude Code skills collection

**Feishu/Lark Integration** ([setup guide](#-feishulark-integration-optional))
- [feishu-claude-code](https://github.com/joewongjc/feishu-claude-code) — Bidirectional Feishu ↔ Claude Code bridge
- [clawdbot-feishu](https://github.com/m1heng/clawdbot-feishu) — Feishu bot for Claude
- [cc-connect](https://github.com/chenhg5/cc-connect) — Multi-platform messaging bridge
- [lark-openapi-mcp](https://github.com/larksuite/lark-openapi-mcp) — Official Lark MCP server

**Community**
- [awesome-agent-skills](https://github.com/VoltAgent/awesome-agent-skills) — Curated list of Claude Code skills (featured)

**Special Thanks — Platform Adaptation**

ARIS wouldn't run on so many platforms without these contributors:

- 🤖 [@Falling-Flower](https://github.com/Falling-Flower) — adapted all ARIS skills for [Codex CLI](https://github.com/openai/codex) using `spawn_agent`
- 🔧 [@No-518](https://github.com/No-518) — ongoing maintenance of the Codex skill set, keeping parity with latest updates
- 🖱️ [@YecanLee](https://github.com/YecanLee) — wrote the [Cursor adaptation guide](docs/CURSOR_ADAPTATION.md) and local GPU setup docs

## License

MIT
