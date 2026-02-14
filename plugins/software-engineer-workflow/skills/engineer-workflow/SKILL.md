---
name: engineer-workflow
description: Completes a software engineering task professionally
---

# Engineer Skill
When completing a software engineering task, the following workflow should take place:
0. Codebase Exploration
1. Define The Task
2. Conduct Research on Existing Solutions
3. Create Implementation Architecture
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

## 0. Codebase Exploration
Before we begin, launch a `repository-explorer` agent with the task of exploring the codebase. They should report back with a structured report on the current codebase. Use it as a reference when continuing, so you don't need to re explore the codebase for each section.

## 1. Defining The Task
When given a task, the first thing to do is to define the problem the task is trying to solve by answering the following questions:
1. Who is the user facing the problem? (Can be developers, executives, IT, business customers, consumers, etc..)
2. Why is the problem occurring?
3. Why does it need to be solved?
4. What is the definition for a minimum solution for this problem?

Try to answer these questions using the task description given to you and what you learned from the code analysis.
If you're not confident in your answers, ask the developer running you follow up questions.

Then, display your answers to these questions to the developer. If they have no need for changes, continue.

## 2. Conduct Research on Existing Solutions
1. Check if a possible solution to the problem already exists in the codebase.
2. Summarize your problem into several simple queries that are optimized for search engines.
3. Search the web with said queries using `WebSearch` and `WebFetch`.
4. Check relevant results such as mail lists, forums, and code repositories
  - Code repositories are the most reliable
  - Mail listings and forum content should be verified
  - Social media is least reliable, but if something is found, the developer should verify the source
5. If a solution exists, understand the solution and analyze it. Think about questions such as:
  - Is the solution well made? Can it be fully applied to your current problem? If not, can you use what was learnt?
  - Can you integrate the solution into the current codebase? Does it make sense to use said solution, given the context of the codebase?
  - What is the form of the solution? Code someone wrote, that you will need to copy and paste? A library is great, but does it come with features that won't be used and will bloat the codebase?
6. Then, summarize what you learnt and request input from the developer running you. If they aren't satisfied, go back to step 1.

## Planning Validation
By now you should have a good understanding of the following:
1. What the problem is, why it exists, and a shallow idea on how it could be implemented
2. What solutions already exist, whether they are good or bad, and if you can use them to solve this problem

Display this a summary of the above to the developer and wait for their input. If they request change, continue answering them and changing the summary until they are satisfied and move on.

## 3. Create Implementation Architecture
Think of a solution, but only on a high level.
Then, create a UML.

## 4. Break Down Architecture Into Sub Tasks
Now that you have a high level architecture for the implementation, break it down into multiple sub tasks (only if needed). Each task should implement part of the architecture. Make sure to note which tasks need to be finished before other tasks, and which tasks can be done in parallel. This is also where you will think about the concrete implementation of the proposed architecture.

Write a markdown file containing the tasks. In the file, make sure there is a flow chart of the order of the tasks that need to be completed.

## 5. Implement Sub Tasks
Start by implementing the first task, and only when finishing it continuing on to the next one. Continue until you finish all tasks.
For each task, document in the same markdown file what you did.

## 6. Review Code
After all tasks have been completed, you need to review the originally planned architecture and the current state of the project in order to understand whether or not you solved the problem. Go over the markdown file which contains the tasks and their implementation. If you find a problem / something was not finished, go back to step 5, and then return to this step again.

After making sure the original task has been solved, we need to review the code. Have a subagent go over the changes and request a code review from him.

## 7. Create Documentation
Now that the problem has been solved, up to date documentation of the codebase is needed in order for future maintainers to have an understanding of why things were changed. A summarization document of this entire workflow should be written.