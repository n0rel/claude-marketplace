---
name: knowledge
description: Use this skill when needing to scan, retrieve, and store knowledge for future use in order to save on repeated tasks such as exploring codebases, documentation, and more
---
# Knowledge Skill

## Knowledge Directories
Knowledge is stored in every codebase under the `./agent-knowledge` directory. This is **always** in the codebases root directory.
When activating this skill, recursively list the directory in order to see its hierarchy and what previous agents haved already stored.

## Storing Knowledge
You should have received the exact information you need to store through your skill activation prompt. Either choose an existing file to update, or generate a new key that will uniquely identify this knowledge. The key should be obvious so that future agents can reference to read or update.
Now, store the knowledge in the `./agent-knowledge/<key>.MD` path as a MARKDOWN file, where <key> is replaced with the knowledge key.

## Retrieving Knowledge
Make sure you have the knowledge key.
Then, read the file at the `./agent-knowledge/<key>.MD` path using the `Read(...)` tool.
