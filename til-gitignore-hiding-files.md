---
  title: "TIL: How .gitignore Made My Files Disappear"
  ---
  Today I learned how easy it is to lose track of your repository's state when jumping between different files and directories, and how a few precise Git diagnostic commands can completely demystify "missing" files.

Here is how the detective work unfolded.

1. The Symptom

I was working on my Python scripts and noticed they completely vanished from git status. At the same time, the root README.md was stubbornly sitting there marked as "untracked," even though I swore I had already committed it.
2. The Hypothesis

I assumed I had messed up my .gitignore rules. I figured a bad pattern was silently swallowing my Python files, while somehow failing to catch things it should be catching.
3. The Diagnostic Commands

To test this, I went full detective mode and ran:
Bash

# Check what changed in my ignore rules
git diff .gitignore

# Peek into Git's blind spot to see exactly what was being filtered out
git status --ignored

4. The Root Cause (The Mystery Solved)

The diagnostics didn't reveal a broken configuration—they revealed the absolute truth of the repository history:

    The Python scripts (hello.py and read_cities.py): They hadn't vanished due to a bug; they were already safely committed and tracked! Because there were no new changes since Thursday, git status naturally had nothing to report. My work was safe and sound.

    The README.md: The README I previously committed in PR #1 was actually the one inside the week-1-basics/ directory, not the root README. The root file showing up as "untracked" was completely correct—it had simply never been committed yet.

    The data/ folder: Running git status --ignored confirmed that the data/ directory was the only thing being ignored. My raw and processed data files were safely excluded, exactly as designed.

5. The Lesson

Don't panic and assume your tools or configurations are broken when files don't appear where you expect them to. Trust the diagnostics.

Running git status --ignored and mapping out your actual commit history will always cut through the assumptions and tell you exactly what Git is seeing.