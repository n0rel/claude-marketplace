---
name: engineer-workflow
description: Completes a software engineering task professionally
---

# Engineer Skill
When completing a software engineering problem, the following workflow should take place:
1. Define the problem
2. Conduct research on existing solutions
3. Create implementation architecture
4. Break down architecture into tasks
5. Implement the tasks
6. Create tests
7. Create documentation


## Background
> "A software engineer applies a software development process, that involves defining, implementing, testing, managing, and maintaining software systems, as well as developing the software development process itself"
(Wikipedia - Software Engineering)[https://en.wikipedia.org/wiki/Software_engineering]

As a software engineer, you're goal is to write, design and maintain software and infrastructure.
You must know how your software interacts with it's environment (both users and infrastructure) and always keep it into consideration.
You must understand that good software is one which is well documented, both in source code and in external documentation, well tested, and is written to make maintainance easier.

## Defining The Problem
When given a task, the first thing to do is define the problem the task is trying to solve:
1. Who is the user facing the problem? (Can be developers, executives, IT, business customers, consumers, etc..)
2. Why is the problem occurring?
3. Why does it need to be solved?
4. How can we, on a theoretical level, solve the problem?

After answering these key questions, we now know enough about the problem to conduct research on existing solutions other software engineers might have already implemented.

## Conduct Research on Existing Solutions
There are tens of thousands of people constantly writing software at the same time. Most of the times, a similar problem you are trying to solve was solved and documented by another engineer. If someone else solved such a problem, they most likely have real world feedback on their solution and their implementation would be better than you implementing it freshly for the first time. This is why it is best practice to conduct research by querying the internet.

1. Summarize your problem into several simple queries that are optimized for search engines
2. Search the web with said queires
3. Check relevant results such as mail lists, forums, and code repositories.
4. If a solution exists, understand the solution and analyze it. Ask questions such as:
  - Is the solution well made? Does it fully apply to your current problem? If not, can you use what was learnt?
  - Can you integrate the solution into the current codebase? Does it make sense to use said solution, given the context of the codebase?
  - What is the form of the solution? Code someone wrote, that you will need to copy and paste? A library is great, but does it come with features that won't be used and will bloat the codebase?
5. Then, summarize what you learnt and let the user running this skill decide for himself
6. If a solution does not exist, continue on.


## Summarize
By now you should have a good understanding of the following:
1. What the problem is, why it exists, and a shallow idea on how it could be implemented
2. What solutions already exist, whether they are good or bad, and if you can use them to solve this problem

Now we will begin our implementation.

## Create Implementation Architecture
Now you will create an architecture for the solution implementation. 
Things like what files to add / remove, what abstractions are needed (if needed), what concrete classes will use these abstractions, what technologies should be added / removed. Start by asking questions about what exists in the codebase currently and what should be changed. For example, if you want to add an endpoint to a backend that does a database call you should ask the following questions:
1. What is the way that endpoints are organized today?
2. Is this something that requires authentication? If it does, how would you make sure the endpoint passes through an existing middleware (if it exists?)
3. Is there another place in the code that performs a database call? If so, does it contain an abstraction? If not, you should add this.

After asking these questions and taking the answers into consideration, create a high level architecture for the implementation.


## Break Down Architecture Into Tasks
Now that you have a high level architecture for the implementation, break it down into multiple tasks (only if needed). Each task should implement part of the architecture. Make sure to note which tasks need to be finished before other tasks, and which tasks can be done in parallel. This is also where you will think about the concrete implementation of the proposed architecture.