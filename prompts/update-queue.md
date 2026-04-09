# Update Review Queue and Regenerate Error Summaries

You are an AI assistant maintaining the spaced repetition queue and error aggregation views.

## Part 1: Rebuild Error Summaries

Read `errors/log.jsonl` and regenerate two files:

### `errors/by-topic.md`

Group errors by topic, then sub_topic. Format:

```markdown
# Error Summary by Topic
Last updated: YYYY-MM-DD

## {{topic}} (X errors, Y unresolved)

### {{sub_topic}} (Z errors)
- E-XXXX [severity] error_type: description (resolved/unresolved)
- ...
```

Sort topics by unresolved error count (descending).

### `errors/by-type.md`

Group errors by error_type. Format:

```markdown
# Error Summary by Error Type
Last updated: YYYY-MM-DD

## {{error_type}} (X total, Y unresolved)

Most affected topics:
1. {{topic}} (N errors)
2. ...

Recent unresolved:
- E-XXXX [YYYY-MM-DD] topic/sub_topic: description
- ...
```

Sort error types by unresolved count (descending).

## Part 2: Queue Maintenance

Read `queue/review-queue.csv` and:

1. **Remove mastered items**: streak >= 3 AND interval_days = 14
2. **Flag overdue items**: next_review < today (these get priority tomorrow)
3. **Identify questions never answered**: in bank.yaml but NOT in review-queue.csv and NOT in any answers.md — these are candidates for future sessions
4. **Print queue stats**:
   - Total items in queue
   - Due tomorrow
   - Due within 3 days
   - Due within 7 days
   - Items by topic (distribution)
   - Unanswered questions count

## Part 3: Generate Weak Topics List

Produce a "weak topics" block the user can paste into the next question-generation session:

```
Weak topics to prioritize:
- {topic/sub_topic}: {N} unresolved errors, last seen {date}
- ...

Spaced review due:
- Q-XXXX: {topic/sub_topic} (due {date})
- ...
```

## Part 4: Weekly Summary (if today is Sunday or end of week)

If today is the last day of the ISO week, create `weekly/YYYY-WXX.md`:

```markdown
# Weekly Review: YYYY-WXX

## Stats
- Sessions this week: N
- Questions attempted: N / M generated
- Completion rate: X%
- Errors logged: N
- Conversation sources: {tool_name: N, ...}

## Topics Covered
- {{topic}}: N questions attempted, M errors

## Persistent Weaknesses
<!-- Topics/sub_topics with errors in 2+ sessions this week -->
- {{sub_topic}}: appeared in N sessions, error types: ...

## Improvements
<!-- Topics where errors were resolved this week -->
- {{sub_topic}}: resolved E-XXXX

## Focus for Next Week
- {{priority_topic_1}}: reason
- {{priority_topic_2}}: reason
```
