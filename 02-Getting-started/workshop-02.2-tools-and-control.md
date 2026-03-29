<img src="https://mfitcontent.blob.core.windows.net/static/MF-logo.png" style="height:64px;margin-right:32px"/>


# Workshop 2.2 — Tools and Control in GitHub Copilot


> **Estimated time: 55 minutes** (5 exercises)
>
> **Difficulty: low to medium** — this workshop teaches you how to use GitHub Copilot safely and deliberately in Visual Studio Code. You will observe tool calls, narrow tool access, approve and reject changes, treat AI output as a first draft, and practice choosing the right interaction mode for the task.

AI-Assisted Development — Getting Started Module

## Terms and Conditions of Use

This training package is proprietary and confidential and is intended exclusively for the uses described in the training materials. Copying or disclosing all or part of the content and/or software included in these packages is prohibited. The contents of this package are for informational and training purposes only and are provided "as is" without warranties of any kind, express or implied, including, but not limited to, the implied warranties of merchantability, fitness for a particular purpose, and non-infringement. The content of the training package, including URLs and other references to Internet websites, is subject to change without notice. Unless otherwise noted, the companies, organizations, products, domain names, email addresses, logos, people, places, and events depicted herein are fictitious and no association with any real company, organization, product, domain name, email address, logo, person, place, or event is intended or should be inferred.

---

## The Scenario

You are ready to let Copilot do more than answer questions. It can read files, search the codebase, propose edits, and in some situations run terminal commands. That power is useful only if you stay in control. In this workshop you will learn to inspect what Copilot is doing, limit the tools it can use, and review every result before accepting it.

> **Remember:** agentic coding is not about giving up control. It is about using automation with clear boundaries, good review habits, and deliberate approvals.

---

## Exercise 1: Understand Tool Usage in Chat

This exercise shows you that Copilot is not just generating text. It can use tools to inspect your environment and act on your repository.

### Prerequisites

Before starting, make sure you have:

- **Visual Studio Code** installed and updated
- **GitHub Copilot** activated and working
- A repository open in VS Code
- Completion of **Workshop 2.1** or equivalent familiarity with chat sessions

### Objective

1. Trigger a response that uses tools
2. Inspect the tool-call summary in chat
3. Understand the difference between reading, searching, editing, and terminal actions

**Step 1 — Ask a prompt that requires inspection**

- In Chat, ask:
  ```
  Find the main entry point of this project and explain how the application starts.
  ```
- This usually requires Copilot to search the codebase or open files

**Step 2 — Inspect the tool summary**

- In the chat transcript, look for the collapsed line that summarizes tool usage
- Expand it to inspect the details
- Note which files Copilot read or searched

**Step 3 — Learn the tool categories**

- As a working mental model, think in four groups:
  - **read**: open files and inspect contents
  - **search**: find code, files, symbols, or usages
  - **edit**: propose or apply file changes
  - **terminal**: run commands in the integrated terminal

**Step 4 — Confirm what happened**

- Answer these questions for yourself:
  - What did Copilot read?
  - What did it search for?
  - Did it propose an edit?
  - Did it try to use the terminal?

### Success Criteria

- [ ] You triggered a response that used one or more tools
- [ ] You expanded the tool-call details in the chat transcript
- [ ] You can explain what Copilot read, searched, or proposed to change

---

## Exercise 2: Control Tool Visibility for the Current Request

This exercise teaches a simple principle: give Copilot only the tools it needs for the task at hand.

### Prerequisites

Before starting, make sure you have:

- The Chat view open in VS Code
- A repository open in the editor

### Objective

1. Open the tool configuration UI for a request
2. Compare a broader tool set with a narrower tool set
3. Learn why narrower permissions reduce risk

**Step 1 — Open Configure Tools**

- In the chat input area, locate **Configure Tools**
- Review which tools are available for the current request

**Step 2 — Try a broader tool set**

- Leave the common tools enabled
- Ask:
  ```
  Find the configuration file that controls logging and suggest one small improvement.
  ```
- Observe which tools Copilot chooses to use

**Step 3 — Try the same task with fewer tools**

- Disable any tool that is not necessary for this task, especially editing or terminal access if you only want analysis
- Ask a similar question again:
  ```
  Find the configuration file that controls logging and explain one improvement you would recommend, but do not make changes.
  ```
- Compare the result and the amount of autonomy Copilot has

**Step 4 — Keep the rule simple**

- If you want explanation, enable reading and search tools
- If you want a small code change, enable editing only when needed
- If you do not need terminal access, keep it off

### Success Criteria

- [ ] You opened the tool configuration UI
- [ ] You ran a task with broader access and a similar task with narrower access
- [ ] You can explain why limiting tools reduces risk and ambiguity

---

## Exercise 3: Use Approvals as Control Points

This exercise shows that approvals are not friction to avoid. They are intentional checkpoints where you confirm that Copilot is still doing the right thing.

### Prerequisites

Before starting, make sure you have:

- A repository open in VS Code
- A small change target available for testing

### Objective

1. Trigger a proposed edit
2. Approve one change and reject one change
3. Recognize approvals as part of the workflow

**Step 1 — Trigger a small edit proposal**

- Ask:
  ```
  In #src/utils, propose one small readability improvement without changing behavior.
  ```
- Wait for Copilot to prepare a concrete change

**Step 2 — Review before approving**

- Read the proposed diff carefully
- Check whether the scope matches the request
- Approve the change only if it is safe and understandable

**Step 3 — Reject a second proposal on purpose**

- Now ask for a different suggestion, or retry on another small file
- Reject the proposal if:
  - it changes behavior
  - it edits more than requested
  - it uses naming or style you do not want

**Step 4 — Optional terminal approval**

- If your environment surfaces terminal approvals, ask for a low-risk terminal action such as identifying the test command for the project
- Review the command before approving it

### Success Criteria

- [ ] You approved at least one proposed change after review
- [ ] You rejected at least one proposed change intentionally
- [ ] You can explain why approvals are checkpoints rather than nuisances

---

## Exercise 4: Treat AI Output as a First Draft

This exercise makes one rule explicit: Copilot can draft useful output quickly, but correctness still depends on your review.

### Prerequisites

Before starting, make sure you have:

- A repository open in VS Code
- At least one file where a small improvement would be plausible

### Objective

1. Generate plausible but imperfect output
2. Review it critically
3. Build a repeatable verification habit

**Step 1 — Ask for a modest change**

- Ask:
  ```
  Suggest a cleaner version of this function and explain why it is better.
  ```
- Choose a function that is short enough to review easily

**Step 2 — Verify the result using a short checklist**

- Before accepting anything, verify:
  - assumptions: did Copilot infer something that is not actually true?
  - naming: are the names clearer or just different?
  - edge cases: does the new version still handle null, empty, or error conditions?
  - placement: is this the right file and the right abstraction level?

**Step 3 — State the rule explicitly**

- Write this down in your notes:
  - **AI-generated output is a first draft, not a final answer**
- Use that rule for explanations, code changes, commands, and summaries

### Success Criteria

- [ ] You reviewed AI output using assumptions, naming, edge cases, and placement
- [ ] You identified at least one thing to verify before accepting the suggestion
- [ ] You can explain why "looks plausible" is not enough

---

## Exercise 5: Choose the Right Mode for the Task

This exercise helps you decide when to use plain chat, a more agentic edit flow, or the Plan agent.

### Prerequisites

Before starting, make sure you have:

- Familiarity with the previous exercises in this workshop

### Objective

1. Classify four common task types
2. Choose between chat, agent-style execution, and `/plan`
3. Explain the reasoning behind the choice

**Step 1 — Classify a simple question**

- Scenario:
  ```
  What does this helper function do?
  ```
- Best fit: **plain chat**

**Step 2 — Classify a local edit in one file**

- Scenario:
  ```
  Rename this variable and simplify this conditional in one file.
  ```
- Best fit: **agent or chat with edit support**, because the task is small and local

**Step 3 — Classify a change across multiple files**

- Scenario:
  ```
  Add a new field to the form, validation logic, submit handler, and summary view.
  ```
- Best fit: **agentic implementation flow**, because the task spans multiple files and requires coordinated work

**Step 4 — Classify an ambiguous architectural task**

- Scenario:
  ```
  Add a user profile area with avatar upload and storage strategy.
  ```
- Best fit: **`/plan` first**, because there are open design decisions and likely cross-cutting changes

**Step 5 — Apply the rule**

- Use **plain chat** for explanation, quick understanding, and low-risk questions
- Use **agentic execution** for bounded edits and multi-file implementation work
- Use **`/plan`** when the task is ambiguous, architectural, or large enough that design should come before edits

### Success Criteria

- [ ] You classified all four scenarios
- [ ] You can justify when to use chat, agentic execution, or `/plan`
- [ ] You can explain why planning should come before editing on ambiguous or architectural tasks

---

## Summary: What You Have Practiced

After this workshop, you should be able to:

| Skill | Outcome |
|------|---------|
| **Read tool usage** | Understand what Copilot inspected before answering |
| **Limit tool access** | Reduce risk by enabling only what the task needs |
| **Use approvals well** | Treat approvals as deliberate control points |
| **Review critically** | Treat AI output as a first draft to verify |
| **Choose the right mode** | Use chat, agentic execution, or `/plan` appropriately |

### Next Step

After completing Workshops 2.1 and 2.2, you are ready for the more advanced modules on planning, custom agents, and skills.
