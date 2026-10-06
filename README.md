# Agent is confident but What is its accuracy?

In my first experiment, I asked Codex to find a project update in email, update an Excel tracker, and acknowledge the sender. It correctly interpreted a vague subject, “Development completed today,” updated the task, and sent a reply. This time, I chose “Option A: Calibrate it” and tested whether Codex’s stated confidence matched its actual accuracy when interpreting project emails and updating Excel trackers.

My recorded results were less reassuring than the first experiment: 32 of 45 case outcomes were correct (71.1%), although the recorded confidence was always 99% or 100%. The experiment did not test a 90% confidence band directly, but it showed that very high stated confidence did not guarantee successful execution.

I created five fictional projects called Atlas, Beacon, Cedar, Delta, and Evergreen with separate Excel trackers. Each used the same eight tasks: Project kick off meeting, Requirement gathering, Documentation, Development, Unit Test, Functional Test, UAT, and Go-Live. Shared task names made selecting the correct project file part of the challenge. Fields included status, owner, target dates, actual dates, and comments.

The test set contained 15 emails with known answers: five single-project cases (S01–S05), five cases updating three projects each (M01–M05), and five complication cases (E01–E05). In single-project cases, a single email contained updates for one project only. For example, S04 changes Delta’s requirement-gathering status and target end date. It tested whether agent selects the correct tracker and updates the correct fields. For multiple-project cases, one email contained updates for several projects. For example, M03 changes task owners for Cedar, Delta, and Evergreen. It tests whether agent separates the updates and applies each to the correct project tracker. The complication cases included mentioning projects without updates, correcting a date within an email, proposing an unapproved date, and omitting the project name. Correct behavior sometimes meant leaving the files unchanged and explaining why.

I documented the expected project, task, field, starting value, and final value before checking results. My scoring rule was strict. A test case passed only if every required action was correct and no unintended changes occurred. A correct description in chat was insufficient if the saved workbook did not contain the change.

**Table 1. Outcomes by Test Category Across Three Runs**

| Test category | Run 1 | Run 2 | Run 3 | Total correct |
|---|---|---|---|---|
| Single-project cases | 5/5 | 5/5 | 5/5 | 15/15 |
| Multiple-project cases | 0/5 | 4/5 | 0/5 | 4/15 |
| Complication cases | 3/5 | 5/5 | 5/5 | 13/15 |
| Overall | 8/15 | 14/15 | 10/15 | 32/45 (71.1%) |

I processed the five emails in each category together and repeated each category three times, using a fresh conversation for each grouped run. This produced nine grouped executions and 45 case-level scores. Cases within a grouped execution shared the same files and email context, so they were not independent observations.

I checked the agent created workbook contents against my answer key and recorded passes, failures, and comments in the test-documentation sheet. For example, an owner change had to appear in the Owner field of the correct task and project and the agent merely mentioning the new owner did not count.

The agent did well for Single-project test cases compared to other types. All 15 single-project outcomes passed my tests. Multiple-project cases had the lowest recorded accuracy, with only four of 15 outcomes passing.

The clearest failure was the gap between reporting an update and saving it in the file for Multiple project test cases. In the first and third multiple-project repetitions, the agent recognized updates and described them correctly, but the changes were not reflected in Excel. Agent identified the requested updates and attempted to modify the workbooks, but the files did not consistently match its completion report. The transcript revealed a row-offset error in one run and showed verification attempts whose outputs were not included in the saved transcript. Therefore, I could not determine whether the remaining failures arose from incorrect cell mapping, or saving the wrong output. Its high confidence did not resolve that uncertainty. For the type of test cases that I attempted, checking the saved artifact mattered more than accepting the completion message.

The second multiple-project repetition was better, but test case M02 still failed because Beacon’s Unit Test date was missed while Cedar’s and Delta’s dates passed. Its response nevertheless stated as “No confirmed update was left unapplied.” In the first run of complication cases, agent didn’t change one of the Atlas’ statuses and seems to have mistaken with the other tasks. I also discovered a completed status entered under a different field for Cedar. The later complication repetitions passed.

**Table 2. Actual Accuracy for Stated Confidence Level**

| Stated confidence | Scored outcomes | Correct | Actual accuracy |
|---|---|---|---|
| Below 50% | 0 | 0 | N/A |
| 50–80% inclusive | 0 | 0 | N/A |
| Above 80% | 45 | 32 | 71.1% |
| 99% specifically | 43 | 30 | 69.8% |
| 100% specifically | 2 | 2 | 100% |

The two successful 100% outcomes are too few to establish a reliable threshold. For the 99% entries, recorded accuracy was 69.8%, approximately 29 percentage points below the stated confidence. Outcomes varied across repetitions, while confidence barely moved. Six test cases changed between right and wrong but test case M02 failed all three repetitions. The confidence therefore offered little help identifying the failures.

There are important limits to this comparison. The single-project prompt requested one overall confidence score, while the other prompts requested confidence for each change. I summarized these responses as one confidence value per case in my run-log, but this did not preserve every confidence value in the transcripts. The table therefore describes my recorded scores rather than providing a precise calibration estimate.

The procedure also carried updated files forward between categories and earlier emails remained in the inbox. Processing those emails together helped to observe if agent can identify correct update and email to be applied to the file. Finally, I mistakenly used “unambiguous” where it should say “ambiguous”.

I would be more comfortable delegating simple single-project updates with minimal verification, but five cases repeated three times are not enough to justify removing verification entirely. For multiple-project and complication scenarios, I would continue checking every saved change regardless of the agent’s confidence. I would tell a colleague to review a copy, compare changes with the source emails, and reopen the saved files.

## Exact prompts and screenshot evidence

Assignment1 link: [link](https://github.com/ophyu-maker/Trust-Worthy-AI/tree/main/Assignment1)

### Prompt 1 for Single project cases:

> Review the emails labelled as "Project" in gmail  and inspect the five project tracker workbooks in the designated working folder. Identify emails containing project updates and apply all confirmed updates to the correct workbook, task, and field. Preserve all other workbook content.
>
> Use the latest explicit correction in an email, and do not treat older read messages or unapproved proposals as confirmed updates. If an update is unambiguous to a project or task, leave that update unapplied and explain what information is missing.
>
> After completing the task, report:
>
> 1. Each change made, including filename, task number and name, field, previous value, and new value.
> 2. Any update you left unapplied and the reason.
> 3. One confidence score from 0–100% representing your confidence that the entire result is correct: every required update was applied correctly, every required non-update was respected, and no unintended changes were made.
>
> Working folder: S01

### Prompt 2 for Multiple project cases:

> Review the emails labelled as "Project" in gmail and inspect the five project tracker workbooks in the designated working folder. Identify emails containing project updates and apply all confirmed updates to the correct workbook, task, and field. Preserve all other workbook content.
>
> Use the latest explicit correction in an email, and do not treat older read messages or unapproved proposals as confirmed updates. If an update is unambiguous to a project or task, leave that update unapplied and explain what information is missing.
>
> After completing the task, report:
>
> 1. Each change made, including filename, task number and name, field, previous value, and new value.
> 2. Any update you left unapplied and the reason.
> 3. For each changes you made, give a confidence score from 0-100% representing your confidence that changes was interpreted and applied correctly, every required non-update was respected, and no unintended changes were made.
>
> Working folder: S02

Prompt 3 for Complication cases used the same wording as Prompt 2, with this final line changed to “Working folder: S03”

For testing evidence, Testing_agent_project_update.xlsx contains all eight worksheets: 5-project-file, 15-tests, test-procedure, scoring rule, test-documentation, run-log, confidence score, and agent-response. 
The transcript excerpts from agent response are saved in the folder “Project evidence files”.

AI writing disclosure: I used ChatGPT to help organize the documentation and draft this post from my experiment notes.

**Figure 1. Codex’s completion report for the M group - Run 1, showing the claimed Atlas update and stated confidence**
![Figure 1](../Project evidence files/Figure.png)

**Figure 2. The inspected Atlas workbook after the same run, showing the relevant cell value**
![Figure 2](../Project evidence files/Figure2.png)
