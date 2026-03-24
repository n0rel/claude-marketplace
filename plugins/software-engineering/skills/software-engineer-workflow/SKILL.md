---
name: software-engineer-workflow
description: Invoke this skill when creating a new feature, bug fix, etc. It will instruct you on how to professionally define, implement and review code.
---

# Engineer Skill
When completing a software engineering task, the following workflow should take place:
0. Codebase Understanding
1. Define The Task
2. Conduct Research on Existing Solutions
3. Create Implementation Architecture (If Needed)
4. Break Down Architecture Into Sub Tasks
5. Implement Sub Tasks
6. Review Code
7. Create Documentation


## Background
> "A software engineer applies a software development process, that involves defining, implementing, testing, managing, and maintaining software systems, as well as developing the software development process itself"
(Wikipedia - Software Engineering)[https://en.wikipedia.org/wiki/Software_engineering]

As a software engineer, you're goal is to write, design and maintain software and infrastructure.
You must know how your software interacts with it's environment (both users and infrastructure) and always take it into consideration when going through the engineering process. 
You must understand that good software is one which is well documented, both in source code and in external documentation, well tested, and is written to make future maintainance easier.

## 0. Codebase Understanding
Before you begin, use the `knowledge` skill to check if there exists a report on the current codebase named `CODEBASE.md`.
If it exists, read the file and then check the commit it is referencing. If the commit is different than the current HEAD, use `git diff {commit} HEAD` to check what has changed, and then update the knowledge file with the updated changes and commit hash.
If no such knowledge exists, use the `explore-codebase` skill.

Now, use the `EnterPlanMode` tool to enter planning mode.

## 1. Defining The Task
When given a task, the first thing to do is to define the problem the task is trying to solve by answering the following questions:
1. Who is the user facing the problem? (Can be developers, executives, IT, business customers, consumers, etc..)
2. Why is the problem occurring?
3. Why does it need to be solved?
4. What is the definition for a minimum solution for this problem?

Try to answer these questions using the task description given to you and what you learned from the code analysis.
If you're not confident in your answers, ask the developer running you follow up questions using the `AskUserQuestion` tool.

Then, display your answers to these questions to the developer using the `AskUserQuestion` tool and ask them if they suggest any modifications. If they have no need for changes, continue.

## 2. Conduct Research on Existing Solutions
1. Check if a possible solution to the problem already exists in the codebase.
2. Summarize your problem into several simple queries that are optimized for search engines.
3. Search the web with said queries using `WebSearch` and `WebFetch`.
4. Check relevant results such as mail lists, forums, documentation and code repositories
  - Code repositories & documentation are the most reliable
  - Mail listings and forum content should be verified
  - Social media is least reliable, but if something is found, the developer should verify the source
5. If a solution exists, understand the solution and analyze it. Think about questions such as:
  - Is the solution well made? Can it be fully applied to your current problem? If not, can you use what was learnt?
  - Can you integrate the solution into the current codebase? Does it make sense to use said solution, given the context of the codebase?
  - What is the form of the solution? Code someone wrote, that you will need to copy and paste? A library is great, but does it come with features that won't be used and will bloat the codebase?
6. Then, summarize what you learnt and ask the developer if the research results are satisfactory using the `AskUserQuestion` tool. If they aren't satisfied, go back to step 1. If they are, continue.

## Planning Validation
By now you should have a good understanding of the following:
1. What the problem is, why it exists, and a shallow idea on how it could be implemented
2. What solutions already exist (if at all), whether they are good or bad, and if you can use them to solve this problem

Verify with yourself that you do indeed understand these two points. If you have any doubts, ask the developer follow up questions using the `AskUserQuestion` tool.

## 3. Create Several Solution Implementations
Think about several implementations for a solution to the task. 
For each implementation, define explicity it's pros and cons, and define a devils advocate line that describes why the implementation is a bad idea.
When finished, display each implementation to the developer using the `AskUserQuestion` tool in order to ask for their preferred implementation.

## 4. Break Down Architecture Into Sub Tasks
Now that you have a chosen implementation, break it down into subtasks using the `TaskCreate` tool.
Make sure to note which tasks need to be finished before other tasks, and which tasks can be done in parallel.

Now, use the `ExitPlanMode` tool to exit planning mode.

## 5. Implement Sub Tasks
Start by creating a new branch using `Bash(git checkout -b)`.
This is where you will implement the tasks. Then list all tasks using the `TaskList` tool and start working on tasks in parallel (if they do not block each other) by using the `TaskGet` tool. Once you are finished with it, use the `TaskUpdate` tool to update your progress and continue on to the next task. Continue until you finish all tasks. Once you are finished with all the tasks, commit your changes to the new branch using `Bash(git add)` tool and `Bash(git commit)` tool with a single sentence as the commit message explaining the changes implemented during this workflow.

## 6. Review Code
Launch a subagent with the task of reviewing the changes made from the commit you made.
Make sure the agent does not review the entire codebase, but only the diff made between the latest commit and the one before.

## 7. Create Documentation
Now, use the `knowledge` skill and check for a knowledge file named `CODEBASE.md`. Update that file with the latest commit and changes you made throughout this skill usage.


## Follow Ups
Once finished, the user will review the task result and may provide subsequent input.
If prompted to change anything, use the `EnterPlanMode` tool to enter planning mode and move back to step 3 (Create Several Solution Implementations) and work your way to step 7 again. Repeat until the user is satisfied with your work on the task.
