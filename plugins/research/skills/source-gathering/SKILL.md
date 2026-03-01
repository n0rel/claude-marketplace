---
name: source-gathering
decription: Gathers sources relevent to a research subject and stores results as tables in a local SQLITE database
allowed-tools:
    - Bash(sqlite3 *)
    - WebSearch
---

# Source Gathering
Before starting, ensure you have the following things:
- An exact, well defined, research subject
- Access to the `Bash(sqlite3 *)` command. If the command isn't accessable, stop executing the skill.

The goal of this skill is to find, evaluate, and compile a list of relavent, high quality sources to be used when conducting research on a subject. At the end of this skill usage, there will be a SQLITE database in the local directory with tables containing data. This will provide a structured, queryable, and easy to navigate source containing all information compiled during this source gathering.

## Define What You Know
Define in a single paragraph what you already know about the research subject. The paragraph should describe the subject as diversely as possible as it will be the basis of the source gathering. If the paragraph is not diverse enough the research conducted will be narrow and contain many overlaps.

## Split Into Semantically Diverse Keywords
Take the paragraph written in the previous phase and split it into semantically diverse keywords.

## Create Search Queries
For each keyword, formulate a search query that will be run in search engines.

## Run Queries
Now, using the `WebSearch` tool, run all formulated search queries **in parallel**. For each search, create a summary.

## Create SQL Tables
With all search results in context, design SQL tables that describe the data and create them in an SQLITE database using the `Bash(sqlite3)` tool. When designing the tables, keep in mind that they serve only as a structure for future ingestion of data.

## Deep Research & Table Populating
Now, for each table, populate it with data **only found in the internet using the `WebSearch` tool**. 
That means you will continuously search the web for data until a satisfactory amount of rows is reached in the table.
To populate rows, use the `Bash(sqlite3)` tool.