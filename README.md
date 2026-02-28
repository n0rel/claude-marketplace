# Claude Marketplace
A marketplace for personal claude code skills & agents

## Usage
Skills do not reference other skills. Before running a main "workflow" skill, activate any domain specific skills beforehand.
For example, the following will force the "python" and "component design principles" skill to be in the current session's context:
```bash
> /python
> /component-design-principles
> /software-engineer-workflow "Add Apache Iceberg implementation"
...
```

## Current Skills
| Plugin | Skill | Description | When to Use |
| -------| ----- | ----------- | ----------- |
| software-engineering | software-engineer-workflow     | Workflow for working on a software engineering task | Adding a new feature to the codebase, refactoring, fixing bugs, adding tests - anything that would be reflected as a "software engineering task". **Always use as the last skill!** Initialize other skills for domain specificity before. |
| software-engineering | component-design-principles    | Reference to SOLID & Component Design Principles | Before writing code, conducting code reviews, designing software components |
| software-engineering | python                         | Reference to modern python development practices | Before writing or reviewing python code |
| software-engineering | knowledge                       | Guide on storing and retrieving codebase specific knowledge for LLM agents future usage | When needing to retrieve past information LLM agents have written down or to note down information for future LLM agents |
