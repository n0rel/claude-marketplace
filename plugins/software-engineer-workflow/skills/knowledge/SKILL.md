---
name: knowledge
description: Instructs model on how to store and retrieve knowledge through keys
---
# Knowledge Skill
A skill defining how to store and retrieve knowledge that is under an agent's domain using a key for every knowledge file.
Should be used when there is no reason to have agents rerun everytime they are spawned, and their information can be loaded dynamically.


## Prerequisites
When invoked, there are 3 prerequisites you **MUST** make sure are passed: 

### 1. Store V.S Retreive

You will first make a distinction: 
1. Was I invoked in order to **store** knowledge?
2. Was I invoked in order to **retreive** knowledge?

### 2. Knowledge Key Name
You should have been given the knowledge key through the skills activation prompt. 
If you weren't given a key and was instructed to store knowledge, generate one that indicatively defines the knowledge being stored in the file.
If you weren't given a key and was instructed to retreive knowledge, do not continue further, and raise the fact that something went wrong while trying to retreive knowledge.

### 3. Agent Name
Make sure you are aware of your defined agent name, as this will be the reference to your knowledge store.
If you don't have a name yet, ask the user to give you one.


## Storing Knowledge
You should have received the exact information you need to store through your skill activation prompt, along with a key that will be used to uniquely define the knowledge.

Now, store the requested knowledge in the `./agent-knowledge/<agent-name>/<key>.MD` path as a MARKDOWN file, where <agent-name> is replaced with your name, and <key> is replaced with the knowledge key.
If the path does not exist, make it using the `Bash(mkdir -p ...)` tool.
When writing the file, use the `Write(...)` tool.


## Retrieving Knowledge
Make sure you have the knowledge key.
Then, read the file at the `./agent-knowledge/<agent-name>/<key>.MD` path using the `Read(...)` tool.