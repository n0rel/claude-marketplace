---
name: engineer-workflow
description: Completes a software engineering task professionally
---

# Engineer Skill
When completing a software engineering task, the following workflow should take place:
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

## 1. Defining The Task
When given a task, the first thing to do is define the problem the task it is trying to solve:
1. Who is the user facing the problem? (Can be developers, executives, IT, business customers, consumers, etc..)
2. Why is the problem occurring?
3. Why does it need to be solved?

Ask the developer running you these questions.

After receiving answers we now know enough about the problem so that we can conduct research on existing solutions other software engineers might have already implemented.

## 2. Conduct Research on Existing Solutions
There are tens of thousands of people constantly writing software at the same time. Most of the times, a similar problem you are trying to solve was solved and documented by another engineer. If someone else solved such a problem, they most likely have experienced real world behavior of their solution and their implementation would be better than you implementing it freshly for the first time. This is why it is best practice to conduct research by querying the internet.

1. Summarize your problem into several simple queries that are optimized for search engines
2. Display those queries to the developer running you and ask for corrections if needed.
3. If given corrections, recontemplate your queries and go back to step 2.
4. Search the web with said queires
5. Check relevant results such as mail lists, forums, and code repositories.
6. If a solution exists, understand the solution and analyze it. Think about questions such as:
  - Is the solution well made? Can it be fully applied to your current problem? If not, can you use what was learnt?
  - Can you integrate the solution into the current codebase? Does it make sense to use said solution, given the context of the codebase?
  - What is the form of the solution? Code someone wrote, that you will need to copy and paste? A library is great, but does it come with features that won't be used and will bloat the codebase?
7. Then, summarize what you learnt and request input from the developer running you. If he replies by asking to search the web again, go back to step 1.
8. If a solution does not exist, continue on.


## Planning Validation
By now you should have a good understanding of the following:
1. What the problem is, why it exists, and a shallow idea on how it could be implemented
2. What solutions already exist, whether they are good or bad, and if you can use them to solve this problem

Display this a summary of the above to the developer and wait for their input. If they request change, continue answering them and changing the summary until they are satisfied and move on.

## 3. Create Implementation Architecture
Now you will create an architecture for the solution's implementation. 
Things like what files to add / remove, what abstractions are needed (if needed), what concrete classes will use these abstractions, what technologies should be added / removed. 
Start by thinking about what exists in the codebase currently and what should be changed. For example, if you want to add an endpoint to a backend that does a database call then answering these questions can aid us:
1. What is the way that the endpoints in the codebase are organized today?
2. Is this something that requires authentication? If it does, how would you make sure the endpoint passes through an existing middleware (if it exists?)
3. Is there another place in the code that performs a database call? If so, does it contain an abstraction?

You now have the basis to create a high level architecture for the implementation.

Create the high level architecture and display it to the developer. Ask for input so that he can provide any tweaks or changes if needed. After changing, ask for input again. Continue this process until the developer is satisfied with the high level architecture.

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