# GitHub issues access

## User

Do you have access to the github issues of the this repo?

## Assistant

I'll check whether this workspace points at a GitHub remote and whether I can read its issues from here.

### Tool Run Terminal Command V2

### Tool Get Mcp Tools

```
{"pattern":"github|issue","toolCallId":"call-68d35bfc-a60c-4b9e-a4c9-a661019baee2-1\nfc_ac3ff1a6-58cd-4fe1-975f-ad6ebc7bb89d_1"}
```

### Tool Read File V2

```
{"path":"\\\\wsl.localhost\\Ubuntu-24.04\\home\\kag18\\ai_fintech\\cmpt983-financebench-rag\\.git\\config"}
```

### Tool Glob File Search

```
{"targetDirectory":"\\\\wsl.localhost\\Ubuntu-24.04\\home\\kag18\\ai_fintech\\cmpt983-financebench-rag","globPattern":"README*"}
```

### Tool Get Mcp Tools

```
{"server":"cursor","toolName":"WebFetch","toolCallId":"call-6467836c-742c-4a9a-bcc2-524e3098c8b7-4\nfc_c9facb8d-6d74-91f4-89ee-9e10e3e4cada_0"}
```

### Tool Web Fetch

```
{"url":"https://github.com/Tarunjeev/cmpt983-financebench-rag/issues","toolCallId":"call-19f5cefc-0e00-4fc9-9275-2961e09bb3cd-5\nfc_d4f6a5bf-5486-9864-9b51-60b558be6f35_0"}
```

## Assistant

Yes. This workspace tracks `Tarunjeev/cmpt983-financebench-rag` on GitHub, and the issues page is public, so I can read it from here.

Right now there is one open issue:

- **#1 — Implement Standard RAG Pipeline**, opened by Tarunjeev on Nov 26, 2026.

I can pull issue titles, bodies, and comments that are public on the web. Creating or editing issues, and anything that needs an authenticated `gh` session, is outside what I can do in this Ask mode session.

## User

Given the following project proposal. Make a plan to split it in 3 parts (one per member to do the work). C:\Users\Kevin\Downloads\CMPT 983 G500 - Project Topic.md

## Assistant

I'll read the project proposal and the current repo so the three-way split matches the actual work.

### Tool Read File V2

```
{"path":"C:\\Users\\Kevin\\Downloads\\CMPT 983 G500 - Project Topic.md"}
```

### Tool Glob File Search

```
{"targetDirectory":"\\\\wsl.localhost\\Ubuntu-24.04\\home\\kag18\\ai_fintech\\cmpt983-financebench-rag","globPattern":"**/*"}
```

### Tool Web Fetch

```
{"url":"https://github.com/Tarunjeev/cmpt983-financebench-rag/issues/1","toolCallId":"call-7001b415-fdb8-4514-8197-517390936fce-8\nfc_6eecad92-3e20-4839-9c8d-599c071cec2e_2"}
```

### Tool Read File V2

```
{"path":"\\\\wsl.localhost\\Ubuntu-24.04\\home\\kag18\\ai_fintech\\cmpt983-financebench-rag\\README.md"}
```

### Tool Read File V2

```
{"path":"\\\\wsl.localhost\\Ubuntu-24.04\\home\\kag18\\ai_fintech\\cmpt983-financebench-rag\\.gitignore"}
```

## Assistant

The repo is still empty aside from a README, and GitHub issue #1 already assigns the standard pipeline to Kevin. I’ll split the proposal into three parallel tracks with a shared data contract so Sabah and Tarun are not blocked on that issue.

### Tool Create Plan

## User

Make a multistep plan to split up the work in GitHub issues. Do not assign them to anyone yet. Only create the issues. Make sure you say which issue has to be solved first and which one comes next (if it is possible show what could be a blocker). Adapt the plan.

## Assistant

The split should become a GitHub issue sequence with blockers, and nobody gets assigned. I’ll update the plan to that and leave the issues uncreated until you approve it.

### Tool Read File V2

```
{"path":"c:\\Users\\Kevin\\.cursor\\plans\\three-way_work_split_2b895ccd.plan.md"}
```

### Tool Edit File V2

## Assistant

The plan now creates ten unassigned GitHub issues and stops there. Nobody is assigned, and no pipeline code is written.

**Solve first:** the shared experiment contract. Every other issue waits on it.

**Then, in order:** the 60–80 question eval set, PDF chunking, and the shared FAISS index. Chunking can be tried on one sample PDF after the contract, but the full chunk file waits on the eval set. The index is the hard gate for both retrievers.

**After the index, in parallel:** System B (standard top-5 retrieval) and System C (metadata filter, then reranking). Inside System C, filtering has to finish before reranking, because the reranker rescores the filtered candidates.

**Off that path:** System A, the no-retrieval baseline, waits only on the contract. It can be written while the PDFs and the index are still in progress.

**Then:** answer generation, which cannot finish until System A, System B, and System C have all landed. Scoring can be coded once the eval set exists, but the final numbers wait on those answer files. Failure labels are last, and they only cover answers that scoring marked wrong.

Existing issue #1 stays as it is, including its current assignee. The plan adds a comment that it has been split into these issues, so it is not worked as one ticket.

## User

Implement the plan as specified, it is attached for your reference. Do NOT edit the plan file itself.

To-do's from the plan have already been created. Do not create them again. Mark them as in_progress as you work, starting with the first one. Don't stop until you have completed all the to-dos.

### Tool Get Mcp Tools

```
{"server":"cursor","toolName":"TodoWrite","toolCallId":"call-9eef645f-e564-4678-9f27-5e69dbe2b469-14\nfc_4c0c1f50-d527-9502-834e-30e3f80dfe98_0"}
```

### Tool Run Terminal Command V2

## Assistant

GitHub is authenticated and only issue #1 exists. I’ll comment on that issue once the new issue numbers exist, and I won’t assign anyone.

### Tool Todo Write

### Tool Run Terminal Command V2

### Tool Run Terminal Command V2

## Assistant

The comment on issue #1 needs the new issue numbers, so I’ll create the ten issues first, then post that comment with the real links.

### Tool Todo Write

### Tool Edit File V2

### Tool Edit File V2

### Tool Edit File V2

### Tool Edit File V2

### Tool Edit File V2

### Tool Edit File V2

### Tool Edit File V2

### Tool Edit File V2

### Tool Edit File V2

### Tool Edit File V2

### Tool Run Terminal Command V2

### Tool Run Terminal Command V2

### Tool Read File V2

```
{"path":"C:\\Users\\Kevin\\AppData\\Local\\Temp\\fb-issues\\rendered-08-generate.md","offset":1,"limit":40}
```

### Tool Read File V2

```
{"path":"C:\\Users\\Kevin\\AppData\\Local\\Temp\\fb-issues\\rendered-09-score.md","offset":1,"limit":30}
```

### Tool Read File V2

```
{"path":"C:\\Users\\Kevin\\AppData\\Local\\Temp\\fb-issues\\rendered-03-chunks.md","offset":1,"limit":25}
```

## Assistant

The ten issues are #2 through #11, all unassigned, and each “Blocked by” line already cites the earlier issue. Next I’ll comment on #1 and fill in the “Blocks” lines.

### Tool Run Terminal Command V2

### Tool Read File V2

```
{"path":"C:\\Users\\Kevin\\.cursor\\projects\\wsl-localhost-Ubuntu-24-04-home-kag18-ai-fintech-cmpt983-financebench-rag\\terminals\\88002.txt","limit":30}
```

### Tool Get Mcp Tools

```
{"server":"cursor","toolName":"AwaitShell","toolCallId":"call-6aa3c5d4-811b-4e87-8e35-620128b8d690-37\nfc_4f288925-90cc-4ea0-afa4-62aac6b8faea_0"}
```

### Tool Await

```
{"taskId":"88002","blockUntilMs":15000,"regex":"ASSIGNEES_AFTER="}
```

### Tool Run Terminal Command V2

### Tool Run Terminal Command V2

### Tool Run Terminal Command V2

### Tool Todo Write

## Assistant

Ten unassigned issues are now on `Tarunjeev/cmpt983-financebench-rag`. Nobody was assigned, and issue #1’s assignee was left unchanged. A comment on [#1](https://github.com/Tarunjeev/cmpt983-financebench-rag/issues/1#issuecomment-7180414133) points at the split so that ticket is not implemented as one task.

**Solve first:** [#2 Define the shared experiment contract](https://github.com/Tarunjeev/cmpt983-financebench-rag/issues/2). Nothing blocks it. Every other issue waits on it.

**Then, in order:**

- [#3 Freeze the stratified evaluation set](https://github.com/Tarunjeev/cmpt983-financebench-rag/issues/3) — blocked by #2
- [#4 Extract text and write metadata chunks](https://github.com/Tarunjeev/cmpt983-financebench-rag/issues/4) — blocked by #2 and #3. A chunker can be tried on one sample PDF after #2, but the full chunk file waits on #3.
- [#5 Build the shared FAISS index](https://github.com/Tarunjeev/cmpt983-financebench-rag/issues/5) — blocked by #4. This is the hard gate for both retrievers.

**After the index, in parallel:**

- [#6 System B standard vector retrieval](https://github.com/Tarunjeev/cmpt983-financebench-rag/issues/6) — blocked by #5
- [#7 System C metadata filter and cross-encoder rerank](https://github.com/Tarunjeev/cmpt983-financebench-rag/issues/7) — blocked by #5. Inside #7, C1 has to finish before C2.

**Off that path:** [#8 System A no-RAG baseline](https://github.com/Tarunjeev/cmpt983-financebench-rag/issues/8) waits only on #2, so it can be written while the PDFs and the index are still in progress.

**Then:**

- [#9 Generate answers for A, B, C1, and C2](https://github.com/Tarunjeev/cmpt983-financebench-rag/issues/9) — the client waits on #2; the full run waits on #6, #7, and #8. A missing retriever blocks only that system’s answer file.
- [#10 Score retrieval and answers](https://github.com/Tarunjeev/cmpt983-financebench-rag/issues/10) — metric code waits on #3; the final numbers wait on #6, #7, and #9
- [#11 Label retriever vs generator failures](https://github.com/Tarunjeev/cmpt983-financebench-rag/issues/11) — blocked by #10, and it blocks nothing else

## User

I need to assigned the issues in the following way: 1st TJ, 2nd Kevin, 3rd Sabah due to their availability. I want everyone to have a similar work load. How should I assign the issues and why?

## Assistant

Assign the critical path in availability order, and keep each person near 10–12 hours. Leave [#1](https://github.com/Tarunjeev/cmpt983-financebench-rag/issues/1) unassigned; the work lives in #2–#11.

| Person | Issues | Rough load |
|---|---|---|
| TJ (available first) | [#2](https://github.com/Tarunjeev/cmpt983-financebench-rag/issues/2) contract, [#3](https://github.com/Tarunjeev/cmpt983-financebench-rag/issues/3) eval set, [#4](https://github.com/Tarunjeev/cmpt983-financebench-rag/issues/4) chunking, [#8](https://github.com/Tarunjeev/cmpt983-financebench-rag/issues/8) System A prompt | ~10–12h |
| Kevin (available second) | [#5](https://github.com/Tarunjeev/cmpt983-financebench-rag/issues/5) FAISS index, [#6](https://github.com/Tarunjeev/cmpt983-financebench-rag/issues/6) System B, [#7](https://github.com/Tarunjeev/cmpt983-financebench-rag/issues/7) System C | ~9–12h |
| Sabah (available third) | [#9](https://github.com/Tarunjeev/cmpt983-financebench-rag/issues/9) generation, [#10](https://github.com/Tarunjeev/cmpt983-financebench-rag/issues/10) scoring, [#11](https://github.com/Tarunjeev/cmpt983-financebench-rag/issues/11) failure labels | ~10–14h |

**TJ goes first because #2, #3, and #4 are a chain and nobody else can start them yet.** #2 blocks every other issue. #3 decides which PDFs exist. #4 produces the chunks. He does them in that order. #8 is the small extra that evens his hours: it only needs #2, so he can write `prompts/qa.txt` immediately after the contract, before returning to the eval set. That also means generation is not later stuck waiting on a one-page prompt.

**Kevin takes the index and both retrievers because that block starts exactly when he becomes available.** #5 is blocked by #4, so it should be the first thing he picks up. #6 and #7 are both blocked only by #5, and both have to finish before answer generation can finish. He does them back to back. #7 is the long one (filter, then rerank), which is why generation stays off his list. Giving #5, #6, or #7 to Sabah would leave the index or a retriever idle until she is free, and that stalls #9.

**Sabah takes the tail because #9, #10, and #11 cannot finish until the retrievers exist.** She can still start early pieces as soon as she is free: the generator client needs only #2, and the metric functions need only #3. The full answer files wait on #6, #7, and #8. The final scores wait on #9. #11 is last and blocks nothing else. The labeling pass is the part that can run long; if it does, TJ and Kevin review a sample of her labels rather than moving the issue.
