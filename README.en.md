# deai-writing-skills

Three Traditional Chinese writing skills for any AI that takes a system prompt, Claude Code included: generate copy without the AI smell, check existing text for it, and distill someone's voice to write in.

**English** | [繁體中文](README.md)

---

## What this is

Three self-contained skills for three writing jobs in Chinese: generate from scratch, check existing text, and distill a person's voice. Each skill is a single `SKILL.md` (plain text) plus a `references/` folder of detail tables.

No platform lock-in. Every `SKILL.md` carries a paste-ready operating prompt, so it runs in any AI that takes a system prompt. If you use Claude Code, you can drop the folders into a skills directory and they auto-load as slash commands, but that is just one option, not a requirement.

This README was written with `deai-write`. If the tool works, its own front page should not read like a machine wrote it.

## The three skills

| Skill | What it does | When to use it |
|---|---|---|
| `deai-write` | Generates a finished Traditional Chinese draft from a topic plus source material. Blog posts, social copy, docs, emails, landing pages, or scripts. The banned patterns never get written in the first place. | You have a topic and material and want new copy that reads human from draft one. |
| `deai-guard` | Audits existing Traditional Chinese text for the AI smell. Produces an evidence report (risk score, five dimensions, per-hit findings), then stops and asks which hits to fix before touching a word. | You have text already written and want it checked, or checked and then rewritten. |
| `deai-voice` | Reverse-analyzes how one person speaks or writes and packs it into a portable voice pack: the full analysis data, a detailed generation prompt, and a built-in de-AI-taste guard, ready to paste into any AI to write in that voice or hand off as a character card. | You want generated text to sound like a specific person, not generic-natural. |

## Four shared ground rules

Every skill holds the same four lines.

1. **No identity verdict.** The skills report "this passage shows N AI-smell features." They never say "a machine definitely wrote this" and never hand you a generation-probability percentage.

2. **Honest boundaries.** No invented facts, numbers, or first-hand experience. Anything that cannot be verified gets marked `[待補]` (to fill) or `[待查]` (to check), left blank rather than faked.

3. **Detection misfires, so it downgrades.** Short text, technical docs, official notices, academic abstracts, non-native Chinese, and SEO copy are handled at a lower strictness, not hard-judged.

4. **Protected spans stay put.** Numbers, terminology, quotes, commands, and proper nouns are never touched during a rewrite. Rewriting means saying it differently, not changing what it says.

## How to use it

Paste it into any AI. Open any `SKILL.md`, copy the operating prompt inside it, and paste that into ChatGPT, Claude, Gemini, or whatever chat AI you use. It follows the same steps. No install, no platform lock-in.

Claude Code makes it smoother. Drop the three folders into a skills directory (global `~/.claude/skills/`, or a project's `.claude/skills/`), restart, and they become `/deai-guard`, `/deai-voice`, and `/deai-write`, triggered automatically by their keywords.

To get the files, clone this repo:

```
git clone https://github.com/donbeeshyvt-jpg/deai-writing-skills.git
```

No build step, no dependencies.

## Minimal usage

### deai-write

Trigger words are 寫一篇貼文, 生成文章, or "draft this without AI taste." It asks for five things before writing: topic, material (and whether each piece is yours, official, or unverified), scene, target reader, and what you want the reader to do. It outputs a build plan first, you confirm, then it writes the full draft with an 11-point plus 5-dimension self-check already run. Gaps come back marked `[待補]` / `[待查]`, never filled with invented numbers.

### deai-guard

Type 檢查 AI 味, 去 AI 味, or "check if this reads like AI" and it starts. First it asks one question: report only, or check-then-fix. Then it audits and hands you a report with a risk score, five dimension scores, and each hit shown with its original sentence, the problem, why it reads like AI, a suggested fix, and where it might be a false positive. Then it stops at a hard gate and asks which hits to fix. Nothing changes until you choose. After the rewrite you get a before/after diff and a residual re-check.

### deai-voice

Say 提煉語氣, 蒸餾說話方式, or "distill this person's voice" to run it. It runs an authorization gate first: only your own writing, content you are authorized to act for, or a named public figure's public material. A trait has to repeat across three or more samples to reach the core layer, and every trait carries a confidence level. You get a portable voice pack: the full reverse-analysis data, a detailed generation prompt (with a built-in de-AI-taste guard and anti-caricature frequency caps), and a character-card view, all in one block you can paste into any AI. The human-readable profile is kept as the review source.

## How they chain

The three are built to hand off to each other.

```
deai-voice  →  deai-write  →  deai-guard
 抽語氣          填進 §0        獨立複審
                風格輸入
```

Distill a voice with `deai-voice`, drop its short prompt into the `風格輸入` (style input) field of `deai-write` §0, generate, then run `deai-guard` for an independent second read. The voice short prompt also feeds `deai-guard` directly: it turns "rewrite this to read more natural" into "rewrite this to sound more like that person."

Each skill stands alone. The chain is optional.

## Repo structure

```
deai-writing-skills/
├── README.md
├── README.en.md
├── LICENSE
├── deai-guard/
│   ├── SKILL.md
│   ├── agents/openai.yaml
│   └── references/
├── deai-voice/
│   ├── SKILL.md
│   ├── agents/openai.yaml
│   └── references/
└── deai-write/
    ├── SKILL.md
    ├── agents/openai.yaml
    └── references/
```

Each `SKILL.md` is the single entry point. Read it and you can start; load the `references/` tables only when you need the detail.

## Limits and known gaps

- Detection is not proof. A low risk score does not certify text as human-written, and a high one does not prove a machine wrote it. The skills rank features, they do not judge authorship.

- Traditional Chinese first. The rules are tuned for Traditional Chinese and mixed Chinese-English text. Other languages are out of scope.

- **Predecessor skills not in this repo.** The three `SKILL.md` files mention earlier sibling skills (`ai-tone-audit`, `de-ai-rewrite`, `voice-style-distill`) and a `NEW_SKILLS_COMPARISON.md`. Those files are not included here. If you see them referenced inside a `SKILL.md`, that is pointing at the predecessors, which live outside this repo.

## License

MIT. See `LICENSE`.
