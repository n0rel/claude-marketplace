---
name: code-review
description: Conduct a code review between two commits.
allowed-tools: 
    - Bash(git diff *)
    - TaskCreate
    - TaskList
    - TaskUpdate
---

# Code Review Skill
This skill contains instructions on how to perform a code review.
Code reviews are always done on a git branch, and uses `git diff` to analyze the changes done.

## Generate Diff
Start by using the `Bash(git diff $0 $1)` tool to generate a diff between the current and previous commit

## Create Tasks
Using the `TaskCreate` tool, create the following reviews:
- Logical: Do the changes make sense?
- Code Standard: Is the new code up to the repository standards?
- Comments: Are all docstrings or comments updated to reflect new changes?
- Security: Are there any obvious security flaws in the code?
- Test Coverage: Are a **minimum** of 50% of components tested?
