# Learning Review Template

A lightweight, AI-powered spaced-repetition review system for any technical subject. Uses **3 AI tools** in a pipeline to generate questions, discuss answers, and track gaps over time.

| Step | Tool | Role |
|------|------|------|
| Generate questions | GPT Pro web | Generate 10 questions from your notes repo |
| Answer questions | GPT / Claude / Gemini web | Multi-round discussion per question (produces share links) |
| Grade & record | Claude Code (this repo) | Grade conversations, log errors, update review queue |

Built on Markdown + YAML + JSONL + CSV. No database, no complex tooling.

---

## Setup

1. **Clone this repo** as your personal review repo (can be private)
2. **Copy config files:**
   ```
   cp config.example.yaml config.yaml
   cp taxonomy.example.yaml taxonomy.yaml
   ```
3. **Edit `config.yaml`** — fill in your source repo path and GitHub URL
4. **Edit `taxonomy.yaml`** — map your topics to source files in your notes repo
5. **Edit `prompts/generate-questions.md`** — replace the repo URL and topic list with yours

---

## Daily Workflow

### Step 1: Generate Questions (GPT Pro web)

1. Open a new GPT Pro conversation
2. Copy the prompt from `prompts/generate-questions.md` and paste it
3. GPT Pro reads your GitHub repo and generates 10 questions
4. If you have weak topics from yesterday's session, paste them too

### Step 2: Answer Questions (GPT / Claude / Gemini web)

1. Pick questions to answer today (you don't have to do all 10)
2. For each question, open a **new conversation** in any AI web tool
3. Type your answer first, then discuss multi-round to deepen understanding
4. When done, **get the share link** for the conversation

### Step 3: Grade & Record (Claude Code)

Open this repo in Claude Code and say:

```
Read prompts/grade-answers.md.

Today's 10 questions from GPT Pro:
[paste the 10 questions here]

I answered 4 questions:
- Q1: https://chatgpt.com/share/xxx
- Q3: https://claude.ai/share/xxx
- Q5: https://g.co/gemini/share/xxx
- Q7: https://chatgpt.com/share/xxx
```

Claude Code will:
- Record questions to `daily/YYYY-MM-DD/questions.md` and `questions/bank.yaml`
- Fetch each share link to read your conversation
- Grade your final understanding (not your first attempt)
- Log errors to `errors/log.jsonl` with full context
- Update `queue/review-queue.csv`
- Generate `daily/YYYY-MM-DD/summary.md`
- Print weak topics you can feed to GPT Pro tomorrow

### (Optional) Step 4: Update Summaries

```
Read prompts/update-queue.md and update the summaries.
```

---

## File Map

```
learning-review/
├── config.yaml                # System settings (copy from config.example.yaml)
├── taxonomy.yaml              # Your topics mapped to source files
├── state.yaml                 # Last reviewed commit, ID counters
│
├── daily/YYYY-MM-DD/          # One folder per session
│   ├── questions.md           #   10 questions (from GPT Pro)
│   ├── answers.md             #   Share links + results
│   └── summary.md             #   Stats, errors, weak topics for tomorrow
│
├── questions/bank.yaml        # All questions ever generated (append-only)
│
├── errors/
│   ├── log.jsonl              # Error log: what you said, what was missing, correct framing
│   ├── by-topic.md            # Errors grouped by topic (regenerated)
│   └── by-type.md             # Errors grouped by error type (regenerated)
│
├── queue/review-queue.csv     # Spaced repetition: 1 → 3 → 7 → 14 days
│
├── weekly/YYYY-WXX.md         # Weekly summaries
│
├── prompts/                   # AI agent instructions
│   ├── generate-questions.md  #   Portable prompt for GPT Pro web
│   ├── grade-answers.md       #   For Claude Code: grade share links
│   └── update-queue.md        #   For Claude Code: rebuild summaries
│
└── templates/                 # File format templates
    ├── questions.md
    ├── answers.md
    └── summary.md
```

---

## Error Recording System

Each error in `errors/log.jsonl` captures:

| Field | Purpose |
|-------|---------|
| `what_user_discussed` | Key statements from your conversation |
| `what_was_missing` | Specific gap in your understanding |
| `correct_framing` | Model answer for future reference |
| `ai_assisted` | Whether you only got it right after heavy AI guidance |
| `share_link` | Link back to the original conversation |

### Error Types

| Type | When to Use |
|------|-------------|
| `concept-unclear` | Can't explain in own words even after discussion |
| `bad-structure` | Correct but disorganized reasoning |
| `missed-tradeoffs` | One-sided analysis, no cost acknowledgment |
| `factual-error` | Wrong number, formula, or mechanism |
| `insufficient-examples` | Abstract without concrete grounding |
| `not-interview-ready` | Technically OK but wouldn't pass interview |

### Grading Philosophy

The grading AI evaluates your **final understanding** at the end of the conversation, not your first attempt. Multi-round discussion is expected and encouraged.
- If you needed heavy AI correction, the error is flagged as `ai_assisted: true`
- If you self-corrected, that counts as a pass

---

## Spaced Repetition

Questions enter the review queue when answered. Intervals: **1 → 3 → 7 → 14 days**.

- Correct (no errors): advance interval
- Errors logged: reset to 1-day interval
- Skipped: no change
- Streak of 3 correct at 14-day: mastered, removed from queue

---

## Closing the Loop

At the end of each grading session, Claude Code outputs a **"Weak Topics"** block. Copy it and paste into GPT Pro's question generation prompt under "Extra Context":

```
errors → weak topics → GPT Pro prioritizes → you practice → errors shrink
```

---

## Tips

- **Don't force 10/10:** Answering 3-5 questions well beats rushing through 10
- **Start conversations with your own answer:** Don't ask the AI to explain first
- **Review `errors/by-topic.md` weekly** to spot persistent weak areas
- **Share links are your study log:** Re-open any conversation to revisit the discussion
