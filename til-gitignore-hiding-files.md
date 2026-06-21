---
title: "TIL: Debugging 'Missing' Files Without Panicking"
---

This week, my Python files vanished from `git status`. I assumed I'd broken something. I hadn't — and the fix was learning to read what Git was actually telling me.

## 1. The Symptom

My Python scripts vanished from `git status`. At the same time, the root `README.md` sat there marked "untracked," even though I swore I'd committed it.

## 2. The Hypothesis

I assumed I'd messed up my `.gitignore` — that a bad pattern was silently swallowing my Python files.

## 3. The Diagnostic Commands

```bash
# Check what changed in my ignore rules
git diff .gitignore

# Peek into Git's blind spot to see what was filtered out
git status --ignored
```

## 4. The Root Cause

The diagnostics didn't reveal a broken config — they revealed the truth of the history:

- **The Python scripts** (`hello.py`, `read_cities.py`) hadn't vanished due to a bug — they were already committed and tracked. With no new changes, `git status` had nothing to report.
- **The README.md** I committed in PR #1 was the one inside `week-1-basics/`, not the root README. The root file showing "untracked" was correct — it had simply never been committed.
- **The data/ folder**: `git status --ignored` confirmed `data/` was the only thing ignored, exactly as designed.

## 5. The Lesson

Don't panic and assume your tools are broken when files don't appear where you expect. Trust the diagnostics. Running `git status --ignored` and mapping your real commit history cuts through assumptions and tells you exactly what Git sees.