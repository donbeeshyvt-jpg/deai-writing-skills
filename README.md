# deai-writing-skills

<!-- Repo URL 尚未建立。所有連結先用佔位符 https://github.com/USERNAME/deai-writing-skills，push 後改成你的 repo URL。 -->

Three Traditional Chinese writing skills for Claude Code and any AI that takes a system prompt: generate copy without the AI smell, check existing text for it, and distill someone's voice to write in.

用繁體中文寫得不像 AI 的三個技能。一個從零生成、一個檢查既有文字、一個把某人的口吻蒸餾成可重用的設定。三個都能裝進 Claude Code 當斜線指令，也能貼進任何吃 system prompt 的 AI。

**English** | [繁體中文](#繁體中文)

---

<a name="english"></a>

## What this is

A monorepo of three self-contained skills. Each folder holds one `SKILL.md` (the only entry point), an `agents/openai.yaml`, and a `references/` folder of detail tables. Drop the folders into your skills directory and they become `/deai-guard`, `/deai-voice`, and `/deai-write`.

This README was written with `deai-write`. If the tool works, its own front page should not read like a machine wrote it.

## The three skills

| Skill | What it does | When to use it |
|---|---|---|
| `deai-write` | Generates a finished Traditional Chinese draft from a topic plus source material. Blog posts, social copy, docs, emails, landing pages, or scripts. The banned patterns never get written in the first place. | You have a topic and material and want new copy that reads human from draft one. |
| `deai-guard` | Audits existing Traditional Chinese text for the AI smell. Produces an evidence report (risk score, five dimensions, per-hit findings), then stops and asks which hits to fix before touching a word. | You have text already written and want it checked, or checked and then rewritten. |
| `deai-voice` | Distills how one person speaks or writes into a reusable voice profile, plus a short prompt you can paste into any AI. | You want generated text to sound like a specific person, not generic-natural. |

## Four shared ground rules

Every skill holds the same four lines.

1. **No identity verdict.** The skills report "this passage shows N AI-smell features." They never say "a machine definitely wrote this" and never hand you a generation-probability percentage.

2. **Honest boundaries.** No invented facts, numbers, or first-hand experience. Anything that cannot be verified gets marked `[待補]` (to fill) or `[待查]` (to check), left blank rather than faked.

3. **Detection misfires, so it downgrades.** Short text, technical docs, official notices, academic abstracts, non-native Chinese, and SEO copy are handled at a lower strictness, not hard-judged.

4. **Protected spans stay put.** Numbers, terminology, quotes, commands, and proper nouns are never touched during a rewrite. Rewriting means saying it differently, not changing what it says.

## Install

The skills are folders. Copy them where Claude Code looks for skills, then restart the session so they load as slash commands.

Global (all sessions):

- macOS / Linux: `~/.claude/skills/`
- Windows: `%USERPROFILE%\.claude\skills\`

Per project (only that project):

- `<project>/.claude/skills/`

### From git

```bash
git clone https://github.com/USERNAME/deai-writing-skills
# push 後把上面的 USERNAME 換成你的 repo
cd deai-writing-skills

# macOS / Linux
mkdir -p ~/.claude/skills
cp -r deai-guard deai-voice deai-write ~/.claude/skills/
```

```powershell
# Windows PowerShell — 先進到 clone 出來的資料夾
cd deai-writing-skills

# 目的地目錄不存在時 Copy-Item 不會自動建，先建好
New-Item -ItemType Directory -Force "$env:USERPROFILE\.claude\skills" | Out-Null

Copy-Item -Path deai-guard,deai-voice,deai-write -Destination "$env:USERPROFILE\.claude\skills" -Recurse
```

Restart the Claude Code session. The commands `/deai-guard`, `/deai-voice`, and `/deai-write` appear once the skills load.

No build step, no dependencies. Each `SKILL.md` also carries a paste-ready operating prompt, so you can run any of the three by pasting it into an AI that has no skill system at all.

## Minimal usage

### deai-write

Trigger words are 寫一篇貼文, 生成文章, or "draft this without AI taste." It asks for five things before writing: topic, material (and whether each piece is yours, official, or unverified), scene, target reader, and what you want the reader to do. It outputs a build plan first, you confirm, then it writes the full draft with an 11-point plus 5-dimension self-check already run. Gaps come back marked `[待補]` / `[待查]`, never filled with invented numbers.

### deai-guard

Type 檢查 AI 味, 去 AI 味, or "check if this reads like AI" and it starts. First it asks one question: report only, or check-then-fix. Then it audits and hands you a report with a risk score, five dimension scores, and each hit shown with its original sentence, the problem, why it reads like AI, a suggested fix, and where it might be a false positive. Then it stops at a hard gate and asks which hits to fix. Nothing changes until you choose. After the rewrite you get a before/after diff and a residual re-check.

### deai-voice

Say 提煉語氣, 蒸餾說話方式, or "distill this person's voice" to run it. It runs an authorization gate first: only your own writing, content you are authorized to act for, or a named public figure's public material. A trait has to repeat across three or more samples to reach the core layer, and every trait carries a confidence level. You get a full profile (six surface dimensions plus eight deep ones) and a short prompt you can paste anywhere.

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

---

<a name="繁體中文"></a>

## 繁體中文

[English](#english) | **繁體中文**

## 這是什麼

一個 monorepo，裝三個自包含的技能。每個資料夾裡一個 `SKILL.md`（唯一入口）、一個 `agents/openai.yaml`、一個放細節表的 `references/`。把資料夾複製到技能目錄，它們就變成 `/deai-guard`、`/deai-voice`、`/deai-write` 三個斜線指令。

這份 README 是用 `deai-write` 寫的。如果工具有效，它自己的門面就不該讀起來像機器寫的。

## 三個技能

| 技能 | 做什麼 | 什麼時候用 |
|---|---|---|
| `deai-write` | 從主題加素材，直接生成一篇繁中成稿。blog、social、docs、email、landing、script 都能寫。禁用句型在生成當下就不寫，不是先寫完再洗。 | 手上有主題和素材，要寫一篇從初稿就沒有 AI 味的新內容。 |
| `deai-guard` | 檢查既有繁中文本的 AI 味，出證據化報告（風險分數、五維度、逐條命中），然後停下來問你要改哪些，收到確認才動手。 | 文章已經寫好，想檢查，或想檢查完再改。 |
| `deai-voice` | 把某人的說話或寫作方式，蒸餾成可重用的 voice profile，外加一段可貼給任何 AI 的短 prompt。 | 想讓生成的文字帶特定人的口吻，不是通用的自然文風。 |

## 四條共用底線

三個技能都守同樣這四條。

1. **不做身分判決。** 技能只說「這幾處呈現 N 個 AI 味特徵」。絕不說「這一定是 AI 寫的」，也不給任何生成機率百分比。

2. **誠實邊界。** 不編造事實、數字、第一手經驗。查不到的具體資訊標 `[待補]` 或 `[待查]`，寧可留白也不填假貨。

3. **偵測會誤判，所以降級。** 短文、技術文件、公文、學術摘要、非母語中文、SEO 制式文，一律降強度處理，不硬判 AI 味。

4. **保護片段不動。** 改寫時不碰數字、術語、引用、命令、專有名詞。改寫是換句話說，不是換內容。

## 安裝

技能就是資料夾。複製到 Claude Code 找技能的地方，重啟 session，它們就載入為斜線指令。

全域（所有 session 可用）：

- macOS / Linux：`~/.claude/skills/`
- Windows：`%USERPROFILE%\.claude\skills\`

專案（只在該專案）：

- `<專案>/.claude/skills/`

### 從 git 安裝

```bash
git clone https://github.com/USERNAME/deai-writing-skills
# push 後把上面的 USERNAME 換成你的 repo
cd deai-writing-skills

# macOS / Linux
mkdir -p ~/.claude/skills
cp -r deai-guard deai-voice deai-write ~/.claude/skills/
```

```powershell
# Windows PowerShell — 先進到 clone 出來的資料夾
cd deai-writing-skills

# 目的地目錄不存在時 Copy-Item 不會自動建，先建好
New-Item -ItemType Directory -Force "$env:USERPROFILE\.claude\skills" | Out-Null

Copy-Item -Path deai-guard,deai-voice,deai-write -Destination "$env:USERPROFILE\.claude\skills" -Recurse
```

複製完重啟 Claude Code session。技能載入後，`/deai-guard`、`/deai-voice`、`/deai-write` 就會出現。

沒有建置步驟，沒有依賴。每個 `SKILL.md` 裡都附一段可直接貼的操作 prompt，所以就算對方是沒有技能系統的 AI，把那段貼進去也能跑。

## 最小用法

### deai-write

觸發詞是 寫一篇貼文、生成文章、或「直接寫出沒有 AI 味的內容」這類話。動筆前它先問五件事：主題、素材（以及每份素材是你本人的、官方的、還是待查的）、場景、目標讀者、你要讀者讀完做什麼。它先出一份施工圖，你確認後才寫全文，交稿時 11 項加 5 維自檢已經跑過。缺口標成 `[待補]` / `[待查]`，不會拿假數字填。

### deai-guard

你打「檢查 AI 味」「去 AI 味」或「幫我看這篇像不像 AI 寫的」，它就啟動。它先問一句：只出報告，還是檢查完幫你改。接著出報告，附風險分數、五維度分數，每一條命中都列出原句、問題、為什麼像 AI、建議改法、可能誤判。然後停在硬性閘門，問你要改哪幾條。沒收到你的選擇，一個字都不動。改完給你 before / after diff，再做一次殘留檢查。

### deai-voice

說「提煉語氣」「蒸餾說話方式」或「抓某人的寫作習慣」就會跑。它先過授權閘門：只收你本人、你被授權代理、或具名公眾人物的公開內容。一個特徵要跨三份以上樣本重複，才進核心層，每條特徵都標信心等級。你會拿到一份完整 profile（表層六維加深層八維）和一段可貼到任何地方的短 prompt。

## 三個怎麼串

三個技能設計成能互相交棒。

```
deai-voice  →  deai-write  →  deai-guard
 抽語氣          填進 §0        獨立複審
                風格輸入
```

用 `deai-voice` 抽出語氣，把它的短 prompt 填進 `deai-write` §0 的「風格輸入」欄，生成，再用 `deai-guard` 獨立複審一次。voice 的短 prompt 也能直接餵給 `deai-guard`，把「改得更自然」對齊成「改得更像某人」。

三個分開用也行，串起來只是可選。

## Repo 結構

```
deai-writing-skills/
├── README.md
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

每個 `SKILL.md` 是唯一入口。讀完就能開始，需要細節再載入 `references/` 的表。

## 限制與已知待處理

- 偵測不是證明。風險分數低不代表這篇一定是人寫的，分數高也不代表一定是機器寫的。技能是在替特徵排序，不是在判作者是誰。

- 以繁中為主。規則是針對繁體中文和中英混排調的，其他語言不在範圍內。

- **前身技能未收錄於本 repo。** 三個 `SKILL.md` 裡提到前身姊妹技能（`ai-tone-audit`、`de-ai-rewrite`、`voice-style-distill`）和一份 `NEW_SKILLS_COMPARISON.md`。這些檔案不在這個 repo 裡。如果你在某個 `SKILL.md` 裡看到它們被提到，那是指前身技能，它們在這個 repo 之外。

## 授權

MIT，見 `LICENSE`。