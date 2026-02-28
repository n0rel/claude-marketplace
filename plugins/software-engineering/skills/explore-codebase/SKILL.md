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


## Tool Usage
When exploring the codebase, use the following tools ONLY:
- Read
- Glob
- Grep

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