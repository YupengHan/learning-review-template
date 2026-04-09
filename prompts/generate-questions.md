# Generate Daily Review Questions

> **This prompt is for GPT Pro web.** Copy-paste it into a new GPT Pro conversation along with the GitHub repo link.

---

## Prompt to paste into GPT Pro:

You are a study coach for [YOUR SUBJECT AREA]. I will give you my GitHub notes repo. Generate **10 review questions** for today's session.

**My repo:** https://github.com/YOUR_USERNAME/YOUR_NOTES_REPO

### Step 1: Check for Recent Updates
- Look at the commit history from the last 48 hours
- Identify which files were added or modified

### Step 2: Generate 10 Questions

| Slot | Source | Count |
|------|--------|-------|
| 1-5 | From recently updated files (if any updates exist in last 48h) | 5 |
| 6-10 | From other topics for broad coverage | 5 |

If there are NO recent updates, generate all 10 from broad coverage.

### Rules
1. Each question must reference the **specific source file** it comes from
2. Mix difficulties: at least 2 foundational, at least 2 advanced, rest intermediate
3. Include at least 1 synthesis question (cross-topic, e.g. "compare X with Y")
4. Each question should have a **follow-up** that goes one level deeper
5. Questions should be answerable in a **systems design interview** style — not trivia
6. Prefer questions about **mechanisms, tradeoffs, and cost models** over definitions
7. For each question, list 3-5 **key points** that a good answer should cover

### Output Format

For each question, output:

```
## Q{N}. [{topic} / {sub_topic}] ({difficulty}) — from: {source_file}

{question text}

**Follow-up:** {deeper follow-up question}

**Key points for a good answer:**
- {point 1}
- {point 2}
- {point 3}
```

### Topic Coverage (pick from these)
<!-- Replace with your own topics from taxonomy.yaml -->
- Topic 1
- Topic 2
- Topic 3

### Extra Context (paste if available)

If the grading AI gave you a list of **weak topics or review queue items**, paste them here so you can prioritize those topics:

```
{PASTE_WEAK_TOPICS_HERE_IF_AVAILABLE}
```

---

## After GPT Pro Generates Questions

Copy all 10 questions and bring them to the **grading AI** (Claude Code) for recording and later grading.

Tell Claude Code:
> Read `prompts/grade-answers.md`. Here are today's 10 questions from GPT Pro: [paste questions]
