# deai-writing-skills

Four Traditional Chinese writing skills that take the AI smell out of AI-written text, and can give it your own voice.

**English** | [繁體中文](README.md)

> The Chinese README is the main page. It was written with the skills in this repo, in a casual Taiwanese "chatty tutorial" voice; this page is its English translation.

---

Can you spot an AI-written article at a glance? It opens with "in today's fast-paced era," it's packed with "it's worth noting that," and it closes by turning everything into a life lesson (you probably wanted to close the tab by now). deai-writing-skills is four Traditional Chinese writing skills built for exactly that problem.

Each one has a clear job: write from scratch, check a draft you already have, capture someone's voice, or write and rewrite articles that need to rank in search. Every skill is a single `SKILL.md` (the skill's instruction file, plain text) plus a `references/` folder of detail tables.

No platform lock-in. Every `SKILL.md` includes an operating prompt (the opening instructions for an AI) that you can paste whole into any chat AI. This page walks you through what each skill does, how to install them, how to chain them, and what our trigger tests found.

---

## The four skills

| Skill | What it does for you | When to reach for it |
|---|---|---|
| `deai-write` | Give it a topic and material and it writes a finished Traditional Chinese draft: blog posts, social posts, docs, emails, landing pages, scripts. Banned patterns never get written in the first place, so there's nothing to scrub afterwards. | You have a topic and material and want new copy that reads human from the first draft. |
| `deai-guard` | Audits text you already have and gives you an evidence-based report (risk score, five dimensions, per-hit findings), then stops and asks which hits to fix. Nothing changes until you say so. | The draft exists and you want it checked, or checked and then fixed. |
| `deai-voice` | Reverse-engineers how a specific person talks and writes, and packs it into a three-file "character card." Paste the first file into any AI and it writes in that voice. | You want AI output that sounds like a particular person, and generic "natural" prose won't do. |
| `deai-writing-seo` | Writes new articles or rewrites existing ones so they rank in search and travel on social. It first confirms what you actually want to say, places keywords by weight, and on rewrites touches only what needs touching so your voice survives. | The article has to climb search results, needs keywords worked in, or a social post is getting no reach. |

All four work on their own, and you can chain them too (see below).

---

## Four shared ground rules

Every skill holds these four lines, no exceptions:

1. No identity verdicts. The skills say "these passages show N AI-smell features." They never say "a machine definitely wrote this," and never hand you a generation-probability percentage.
2. Blank beats fake. No invented facts, numbers, or first-hand experience. Gaps get marked `[待補]` (to fill) or `[待查]` (to check).
3. Detection misfires, so it downgrades. Short text, technical docs, official notices, academic abstracts, non-native Chinese, and SEO copy are judged more leniently, not hard-judged.
4. Protected spans stay put. Numbers, terminology, quotes, commands, and proper nouns are never touched during a rewrite; only the wording changes.

---

## Installing: three ways

### Option 1: paste the prompt (works in any AI)

No install at all, and the fastest way in:

1. Open any `SKILL.md` and find the paste-ready operating prompt section (可直接貼的操作 prompt).
2. Copy it whole into ChatGPT, Claude, Gemini, or whichever chat AI you use.
3. Hand it your topic, draft, or samples, and it follows the same steps.

Quick tip: `deai-voice` normally produces three files. In a chat that can't create files, it gives you the three parts in the conversation instead; just save them yourself.

### Option 2: install into Claude Code (it starts on its own when you mention a trigger)

Claude Code reads skills from its skills folder automatically. Once they're installed, a trigger phrase (like "check this for AI smell") starts the matching skill on its own, or you can call one with a slash command such as `/deai-guard`.

Step 1, clone the repo anywhere:

```
git clone https://github.com/donbeeshyvt-jpg/deai-writing-skills.git
```

Step 2, copy the four skill folders into the skills folder.

macOS / Linux:

```
mkdir -p ~/.claude/skills && cp -r deai-writing-skills/deai-* ~/.claude/skills/
```

Windows (PowerShell):

```
New-Item -ItemType Directory -Force "$HOME\.claude\skills" | Out-Null
Copy-Item -Recurse deai-writing-skills\deai-* "$HOME\.claude\skills\"
```

To use them in a single project only, put them in that project's `.claude/skills/` instead.

Step 3, check that it looks like this:

```
~/.claude/skills/
├── deai-write/SKILL.md
├── deai-guard/SKILL.md
├── deai-voice/SKILL.md
└── deai-writing-seo/SKILL.md
```

⚠️ Important: don't clone the whole repo straight into the skills folder. With the extra level, `~/.claude/skills/deai-writing-skills/deai-write/SKILL.md`, Claude Code finds none of the skills (we tested it: all four disappear).

New skills usually show up without a restart; if they don't, restart Claude Code once.

### Option 3: install into Codex

Codex (OpenAI's coding agent) uses the same skill format. Per the [official Codex docs](https://developers.openai.com/codex/skills), the personal skills folder is `~/.agents/skills/`; copy the four folders in there. The `agents/openai.yaml` file in each skill holds the name and description Codex shows in its interface. (For Codex we've only checked the official docs, not tested it on a real machine.)

---

## Using each skill

### deai-write: write from scratch

Phrases like 寫一篇貼文 (write a post), 生成文章 (generate an article), or "write it without the AI smell" start it. Before writing, it asks five things: topic, material (and whether each piece is yours, official, or unverified), scene, target reader, and what you want the reader to do afterwards. It gives you a build plan first and writes the full draft once you confirm; an 11-point check and a 5-dimension self-review have already run by the time you get it.

### deai-guard: check first, fix only when you say so

Type 檢查 AI 味 (check for AI smell), 去 AI 味 (remove the AI smell), or "does this read like AI?" and it starts. First it asks: report only, or check and then fix? Every hit in the report lists the original sentence, the problem, why it reads like AI, a suggested fix, and when it might be a false positive. Then it stops and waits for you to pick which hits to fix (without your choice, not a single word changes). After the rewrite you get a before/after comparison plus a residual check.

### deai-voice: pack a voice into a character card

To capture someone's voice, say 提煉語氣 (distill a voice), 蒸餾說話方式 (distill how someone talks), or "capture how this person writes." It runs an authorization gate first: only your own writing, content you're authorized to act for, or a named public figure's public material. A trait has to show up in three or more samples to count.

You end up with a character-card folder. Paste the first file, `1-說話格式.md` (speaking format), into any AI and it writes in that voice. The second, `2-資料庫.md` (database), is the full voice analysis with an original example for every item. The third, `3-來源索引.md` (source index), records where each sample came from and when it was collected; it's for people only and never gets fed to the AI.

### deai-writing-seo: write articles that rank

When an article needs to climb the rankings, give it SEO 優化 (optimize for SEO), 改寫成 SEO 文章 (rewrite as an SEO article), 融入關鍵字 (work in keywords), or 幫我下標題 (write me a title). Before starting, it asks for the task type (new article, SEO rewrite of an old draft, social adaptation, or light touch-up), core keyword, audience, platform, and goal.

On a rewrite, it first reports its guess at what the piece is trying to say and achieve, and asks you to confirm; a wrong guess means everything gets re-evaluated. Numbers, steps, stance, and pet phrases are locked before anything changes. It reads search intent by looking at what actually ranks on page one for the keyword (no guessing), places keywords by weight, and keeps each page to one core intent. Algorithm rules are treated as direction, never stated as fact.

---

## How they chain

They're built to hand off to each other:

```
deai-voice  →  deai-write        →  deai-guard
 capture         write with the     independent
 the voice       style input        second read

deai-voice  →  deai-writing-seo  →  deai-guard
 capture         write articles     independent
 the voice       that need to rank  second read
```

In short: turn a voice into a character card with `deai-voice`, then paste its first file into the style input (風格輸入) field of `deai-write`; articles that need to rank go to `deai-writing-seo` instead. When the draft is done, run it past `deai-guard`, an independent second pair of eyes.

Each skill works fine on its own; chaining is just one more option.

---

## Trigger test results

In September 2026, on Claude Code 2.1.281 with Opus 5.5 and only these four skills installed, we tested triggering with 33 realistic requests, 2 runs each, 66 runs total.

- The 25 requests that should trigger a skill: all 50 runs picked the right one. Four of them were mixed requests where two skills could fit (like "remove the AI smell and work in a keyword," or "write an SEO article in my voice"), and those also went to the more specialized skill.
- 8 decoys (translation, typo-only fixes, judging whether an image is AI-made, technical SEO settings, and so on): 16 runs, 0 false triggers.
- One exception to watch for: for a "just polish this" on a few short lines, the AI sometimes decides to edit directly without calling `deai-guard` (more often in setups with many skills installed). For the full flow, type `/deai-guard`.

(Tested on one machine with one model; results may differ on other models or versions.)

---

## Repo structure

```
deai-writing-skills/
├── README.md
├── README.en.md
├── LICENSE
├── deai-write/
│   ├── SKILL.md            ← entry point; read it and you can start
│   ├── agents/openai.yaml  ← display settings for the Codex interface
│   └── references/         ← detail tables, loaded only when needed
├── deai-guard/             (same layout)
├── deai-voice/             (same layout)
└── deai-writing-seo/       (same layout)
```

---

## Limits and known behavior

- Detection is not proof. A low risk score doesn't certify human writing, and a high one doesn't prove a machine wrote it. The skills rank features; they don't judge authorship.
- Traditional Chinese first. The rules are tuned for Traditional Chinese and mixed Chinese-English text; other languages are out of scope.
- Algorithm rules are mostly speculative. The social and search algorithm rules `deai-writing-seo` works with have no complete official documentation, so the skill treats them as direction only.
- Short "polish this" requests don't always trigger `deai-guard` (see the test results); type `/deai-guard` for the full flow.
- Skill upload to claude.ai (the web app) hasn't been tested yet; to use the skills there, pasting the prompt (Option 1) is the safest bet.

---

## How this README was written

The Chinese README was written with this repo's own skills. `deai-voice` distilled a "chatty web tutorial" (網文聊天體) character card from 39 public Taiwanese tutorial and pop-science articles (a genre voice, not any particular author). `deai-write` wrote the page with that card as its style input, and `deai-guard` gave it an audit pass. This English page is a translation.

So if you hit a passage that reads like AI, that's on the skills (open an issue and call it out). Once they're installed, grab something you wrote recently and hand it to `deai-guard`.

---

## License

MIT. See `LICENSE`.
