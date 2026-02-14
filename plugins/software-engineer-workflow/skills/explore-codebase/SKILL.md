---
name: explore-codebase
description: Explores the codebase completely, understanding directory heirarchy, what classes exist, what functions exist, dependencies & technologies
---

# Explore Codebase

## Overview
The goal of this skill is to come to a full understanding of the layout of the given codebase.
This includes the full understanding of:
- Directory heirarchy
- File namesin 
- What objects and functions exist
- Dependencies


After understanding the above, a markdown file that contains all the information gathered should be returned to the invoker of this skill.


## Tool Usage
When exploring the codebase, use the following tools ONLY:
- Read
- Glob
- Grep

When writing your report, use the following tools ONLY:
- Write


## Directory Heirarchy
When trying to understand the layout of the codebase, assign a description to each directory in the codebase. 
Use information about the type of files that the directory contains, their names, their content, and their relation to the context
of the codebase (for example: a pyproject.toml probably means the project contains python code)


## File Names
For every file, hypothesize it's usage by reading its name. Then, when reading the file, confirm whether you were correct or not. This should
help you think about the files' responsibility in the codebase.


## What Objects And Functions Exist
When reading files, take note of what objects and functions exist, their docstrings, and when they are used in other parts of the codebase.


## Dependencies
When exploring, take note of what extenral dependencies are used throughout the codebase. This could be things like databases, packages, message queues, API's, DLLs, etc..


## Report Creation
Once you finish understanding how a codebase is structured, return a report written in markdown to the invoker.
Style it like so:


| Document Part | Description |
|-----------------|-----------------|
| Header | The name of the codebase |
| Commit | The GIT commit of when the report was written. This is so that future readers of the report can check the git history from this commit onwards to see changes, without looking throughout the entire codebase all the time |
| Description | Short description about the codebase containing what languages are used, how many files exist, the main functionality of the code |
| File Structure | The directory heirarchy. Each directory should be accompanied with a brief description of what it contains, and how often it is used
| Objects | What objects exist and the path they are contained in |
| Functions | What functions exist and the path they are contained in |
| Dependencies | What dependencies does the project need in order to function, and what their usage is |