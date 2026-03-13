<img src="https://mfitcontent.blob.core.windows.net/static/MF-logo.png" style="height:64px;margin-right:32px"/>


# Workshop 3 — Customizing GitHub Copilot in VS Code: From Instructions to Agents

> **Estimated time: 120 minutes** (7 exercises)
>
> **Difficulty: intermediate** — this workshop walks you through the full customization of GitHub Copilot in VS Code. You will start from always-on instructions and progress all the way to configuring custom agents, skills, hooks, and plugins. Each exercise corresponds to one of the customization options available in the platform.

AI-Assisted Development — Module 3

## Terms and Conditions of Use

This training package is proprietary and confidential and is intended solely for the uses described in the training materials. Copying or disclosing all or part of the content and/or software included in such packages is prohibited. The contents of this package are for informational and training purposes only and are provided "as is" without warranties of any kind, express or implied, including, but not limited to, the implied warranties of merchantability, fitness for a particular purpose, and non-infringement. The content of the training package, including URLs and other references to Internet websites, is subject to change without notice. Unless otherwise noted, the companies, organizations, products, domain names, email addresses, logos, people, places, and events depicted herein are fictitious and no association with any real company, organization, product, domain name, email address, logo, person, place, or event is intended or should be inferred.

---

<introduction>

GitHub Copilot is not just a generic assistant: it is a composable system you can tailor to your project, your team, and your workflows. In this workshop we will explore all the customization options available in Visual Studio Code, each with a dedicated hands-on exercise.

The following table summarizes the seven customization types we will cover:

| # | Goal | Customization | When it activates |
|---|------|---------------|-------------------|
| 1 | Apply coding standards everywhere | Always-on Instructions | Automatically included in every request |
| 2 | Different rules for different file types | File-based Instructions | When files match a pattern or description |
| 3 | Reusable tasks to run repeatedly | Prompt Files | When you invoke a slash command |
| 4 | Package multi-step workflows with scripts | Agent Skills | When the task matches the skill description |
| 5 | Specialized AI persona with tool restrictions | Custom Agents | When you select it or another agent delegates to it |
| 6 | Automate tasks at agent lifecycle points | Hooks | When the agent reaches a matching lifecycle event |
| 7 | Connect to external APIs or databases | MCP | When the task matches a tool description |

Each exercise is self-contained, but the order is designed to progressively build your understanding of the customization system.

</introduction>

---

## Exercise 1: Always-on Instructions — Global Coding Standards

This exercise teaches you how to create a `copilot-instructions.md` file that is automatically applied to every Copilot Chat interaction in your workspace. It is the starting point for ensuring that all generated code follows your project conventions.

### Prerequisites

Before you begin, make sure you have:

- Visual Studio Code installed (version 1.100+)
- The GitHub Copilot extension active with a valid plan (Free, Pro, Business, or Enterprise)
- A Node.js/TypeScript project open in VS Code (or create an empty one with `npm init -y && npx tsc --init`)

### Objectives

1. Create the `.github/copilot-instructions.md` file at the workspace root
2. Write style instructions and project conventions in Markdown format
3. Verify that Copilot automatically applies the instructions in every response


**Step 1 — Create the folder and file**

- Open the VS Code integrated terminal (`Ctrl+`` `)
- Run the following commands:
  ```bash
  mkdir -p .github
  touch .github/copilot-instructions.md
  ```
- Open the newly created file in the editor

**Step 2 — Write the always-on instructions**

- Paste the following example instructions into the file (adapt them to your actual project):
  ```markdown
  # Project Standards

  ## General Conventions
  - Use TypeScript for all new JavaScript code
  - Prefer functional patterns and immutability when possible
  - Write JSDoc comments for all public APIs
  - Use 2 spaces for indentation, never tabs

  ## Naming
  - Variables and functions: camelCase
  - Classes and interfaces: PascalCase
  - Constants: UPPER_SNAKE_CASE
  - Files: kebab-case.ts

  ## Error Handling
  - Always handle errors with try-catch in async blocks
  - Return meaningful error messages, never generic strings
  - Log errors with a structured logger (not console.log)

  ## Testing
  - Include unit tests for all business logic
  - Use Jest as the testing framework
  - Aim for at least 80% code coverage
  ```

**Step 3 — Verify it works**

- Open Copilot Chat (`Ctrl+Alt+I`)
- Ask: *"Create a function that reads a JSON file and returns the parsed data"*
- Observe that Copilot uses TypeScript, camelCase, error handling with try-catch, and JSDoc comments — without you having to specify any of this in the prompt

**Step 4 — Verify via Diagnostics**

- Right-click in the Chat view and select **Diagnostics**
- Verify that the `copilot-instructions.md` file appears among the loaded instructions
- If it does not appear, check that the file is in the correct location: `.github/copilot-instructions.md`


### Success Criteria

- [ ] The `.github/copilot-instructions.md` file exists at the workspace root
- [ ] Copilot generates code that follows the conventions written in the file (TypeScript, camelCase, JSDoc, try-catch)
- [ ] The Diagnostics view shows the file among the active instructions

---

## Exercise 2: File-based Instructions — Type-Specific Rules

This exercise teaches you how to create instruction files scoped to specific file patterns, such as `.tsx`, `.test.ts`, or `.css`. Unlike always-on instructions, these activate only when you are working on files that match the specified pattern.

### Prerequisites

Before you begin, make sure you have:

- Completed Exercise 1
- A `.github/instructions` folder in the workspace (or the `chat.instructionsFilesLocations` setting configured)

### Objectives

1. Create a `*.instructions.md` file with the `applyTo` property in the YAML frontmatter
2. Define specific rules for React `.tsx` files
3. Verify that the instructions only activate on files matching the pattern


**Step 1 — Create the folder structure**

- Run in the terminal:
  ```bash
  mkdir -p .github/instructions
  ```

**Step 2 — Create instructions for React components**

- Create the file `.github/instructions/react-components.instructions.md`:
  ```markdown
  ---
  applyTo: "**/*.tsx"
  description: "Conventions for React components in TypeScript"
  ---

  # Rules for React Components (.tsx)

  - Always use functional components with arrow functions and named exports
  - Define props with a dedicated TypeScript interface (Props suffix)
  - Use React.FC only when necessary for children
  - Prefer useState and useReducer for local state
  - Extract complex logic into separate custom hooks
  - Each component must have its own file — one file = one component
  - Use Tailwind CSS for styling, no CSS-in-JS
  ```

**Step 3 — Create instructions for test files**

- Create the file `.github/instructions/testing.instructions.md`:
  ```markdown
  ---
  applyTo: "**/*.test.ts"
  description: "Conventions for test files"
  ---

  # Rules for Tests

  - Use the AAA pattern (Arrange, Act, Assert) in every test
  - Name tests following the pattern: "should [expected behavior] when [condition]"
  - Mock external dependencies, never internal modules
  - Each test file tests a single module
  - Include at least one test for the error case
  ```

**Step 4 — Verify conditional activation**

- Open a `.tsx` file and ask Copilot Chat: *"Create a Card component with title and description"*
- Verify it follows the React rules (functional component, Props interface, Tailwind)
- Now open a `.ts` file (not `.tsx`) and make the same request: the React rules **should not** activate
- Open a `.test.ts` file and ask: *"Write tests for an add(a, b) function"*
- Verify it uses the AAA pattern and the correct naming convention


### Success Criteria

- [ ] At least two `*.instructions.md` files exist with `applyTo` in the frontmatter
- [ ] React rules apply only when working on `.tsx` files
- [ ] Testing rules apply only when working on `.test.ts` files
- [ ] The Diagnostics view confirms which instructions are active for each context

---

## Exercise 3: Prompt Files — Reusable Tasks with Slash Commands

This exercise teaches you how to create reusable prompt files that you can invoke as slash commands in Copilot Chat. They are ideal for repetitive tasks such as scaffolding components, generating documentation, or preparing pull requests.

### Prerequisites

Before you begin, make sure you have:

- Completed Exercises 1 and 2
- The `.github/prompts` folder in the workspace

### Objectives

1. Create a `.prompt.md` file with YAML frontmatter
2. Use input variables (`${input:...}`) to parameterize the prompt
3. Invoke the prompt as a slash command and verify the result


**Step 1 — Create the prompts folder**

- Run in the terminal:
  ```bash
  mkdir -p .github/prompts
  ```

**Step 2 — Create a component scaffolding prompt**

- Create the file `.github/prompts/create-component.prompt.md`:
  ```markdown
  ---
  agent: 'agent'
  description: 'Generate a new React component with tests and storybook'
  ---

  Create a new complete React component with the following specifications:

  Component name: ${input:componentName:Enter the component name}
  Type: ${input:componentType:Component type (ui, layout, form, page)}

  For each component, generate:
  1. The component file (.tsx) following the instructions in [react-components](../instructions/react-components.instructions.md)
  2. The test file (.test.tsx) following the instructions in [testing](../instructions/testing.instructions.md)
  3. An index.ts file for the export

  Folder structure:
  src/components/${input:componentName}/
  ├── ${input:componentName}.tsx
  ├── ${input:componentName}.test.tsx
  └── index.ts
  ```

**Step 3 — Create a code review prompt**

- Create the file `.github/prompts/code-review.prompt.md`:
  ```markdown
  ---
  agent: 'ask'
  description: 'Perform a structured review of the selected code'
  ---

  Perform a code review of the provided code. Structure your feedback into the following sections:

  ## Analysis
  - Code quality and adherence to project standards
  - Potential bugs or security issues
  - Performance and possible optimizations

  ## Suggestions
  For each issue found, indicate:
  - Severity (high / medium / low)
  - Affected line or code block
  - Concrete suggestion for the fix

  ## Verdict
  Indicate whether the code is ready for merge or needs changes.
  ```

**Step 4 — Use the prompt files**

- Open Copilot Chat
- Type `/create-component` in the input bar: the prompt will appear among the available commands
- Copilot will ask you for the parameter values (component name, type)
- Verify that the result generates the complete structure with all files

**Step 5 — Generate a prompt with AI (optional)**

- In the chat, type `/create-prompt` and describe the task you want to automate, for example: *"a prompt to generate unit tests from a source file"*
- Copilot will generate the `.prompt.md` file for you
- Review the result and save the file in the `.github/prompts` folder


### Success Criteria

- [ ] At least two `.prompt.md` files exist in the `.github/prompts` folder
- [ ] The `/create-component` prompt is visible as a slash command in Copilot Chat
- [ ] When invoked, Copilot asks for input parameters and generates the expected result
- [ ] The prompt references existing instruction files via Markdown links

---

## Exercise 4: Agent Skills — Multi-Step Workflows with Scripts and Resources

This exercise teaches you how to create an Agent Skill — a folder containing instructions, scripts, and resources that Copilot automatically loads when the task matches the skill description. Skills are portable and also work with Copilot CLI and the Copilot coding agent.

### Prerequisites

Before you begin, make sure you have:

- Visual Studio Code version 1.108+ (Agent Skills were introduced in December 2025)
- Completed the previous exercises
- Node.js installed (for the example scripts)

### Objectives

1. Create a skill folder structure with a `SKILL.md` file and resources
2. Define the description in the frontmatter for automatic activation
3. Verify that Copilot loads the skill when the task is relevant


**Step 1 — Create the skill folder structure**

- Run in the terminal:
  ```bash
  mkdir -p .github/skills/api-endpoint
  ```

**Step 2 — Create the SKILL.md file**

- Create the file `.github/skills/api-endpoint/SKILL.md`:
  ```markdown
  ---
  name: api-endpoint
  description: >
    Generate a complete REST API endpoint with controller, service, validation,
    and tests. Use this skill when asked to create a new API,
    a new endpoint, or a new route.
  ---

  # REST API Endpoint Creation

  This skill generates a complete API endpoint following the project architecture.

  ## When to use this skill
  - Creating a new REST endpoint
  - Adding a route to an existing controller
  - Generating boilerplate for a complete CRUD

  ## Structure to generate
  For each endpoint, create the following files:
  1. Controller: `src/controllers/{name}.controller.ts`
  2. Service: `src/services/{name}.service.ts`
  3. Validation: `src/validators/{name}.validator.ts`
  4. Tests: `src/controllers/{name}.controller.test.ts`

  ## Controller template
  Follow the template in [controller-template](./controller-template.ts)

  ## Conventions
  - Use decorators for input validation
  - Handle errors with centralized middleware
  - Every endpoint must have inline OpenAPI documentation
  - Tests must cover the cases: success, validation error, not found, server error
  ```

**Step 3 — Add a reference template**

- Create the file `.github/skills/api-endpoint/controller-template.ts`:
  ```typescript
  import { Request, Response, NextFunction } from 'express';
  import { validateRequest } from '../middleware/validation';

  /**
   * @openapi
   * /api/{resource}:
   *   get:
   *     summary: Retrieve the list of resources
   *     responses:
   *       200:
   *         description: List retrieved successfully
   */
  export const getAll = async (
    req: Request,
    res: Response,
    next: NextFunction
  ): Promise<void> => {
    try {
      // TODO: implement service logic
      res.json({ data: [] });
    } catch (error) {
      next(error);
    }
  };
  ```

**Step 4 — Verify automatic activation**

- Open Copilot Chat in agent mode
- Ask: *"Create an API endpoint for managing users with full CRUD"*
- Copilot should recognize the `api-endpoint` skill and follow the defined structure
- Alternatively, type `/api-endpoint` in the chat to manually activate the skill
- Check the Diagnostics view to confirm the skill was loaded


### Success Criteria

- [ ] The `.github/skills/api-endpoint/` folder contains `SKILL.md` and at least one resource file
- [ ] The `SKILL.md` has a frontmatter with `name` and `description`
- [ ] Copilot automatically loads the skill when you ask to create an API endpoint
- [ ] The generated code follows the structure and conventions defined in the skill

---

## Exercise 5: Custom Agents — Specialized AI Persona

This exercise teaches you how to create a custom agent — a specialized "persona" with its own set of instructions, available tools, and preferred model. Custom agents are ideal for roles such as security reviewer, database admin, or planner.

### Prerequisites

Before you begin, make sure you have:

- Visual Studio Code version 1.108+
- Completed the previous exercises

### Objectives

1. Create an `.agent.md` file with frontmatter defining tools, model, and handoffs
2. Select the custom agent from the agent picker in the Chat view
3. Verify that the agent operates with the defined restrictions and behavior


**Step 1 — Create the agents folder**

- Run in the terminal:
  ```bash
  mkdir -p .github/agents
  ```

**Step 2 — Create a "Security Reviewer" agent**

- Create the file `.github/agents/security-reviewer.agent.md`:
  ```markdown
  ---
  name: Security Reviewer
  description: >
    Reviews code from a security perspective, identifying vulnerabilities,
    authentication issues, injection flaws, and data exposure.
  tools: ['search/codebase', 'search/web']
  model: 'Claude Sonnet 4.6'
  ---

  # Security Reviewer Instructions

  You are an application security expert. Your role is to review code
  without making direct changes.

  ## Analysis Focus
  - OWASP Top 10 vulnerabilities (injection, broken auth, XSS, CSRF, etc.)
  - Secure handling of credentials and secrets
  - Input validation and sanitization
  - Sensitive data exposure in API responses
  - Security configurations (CORS, headers, rate limiting)

  ## Report Format
  For each issue found, report:
  - **Severity**: Critical / High / Medium / Low / Info
  - **OWASP Category**: which Top 10 category applies
  - **Location**: affected file and line (or block)
  - **Description**: what is wrong and why it is a risk
  - **Remediation**: how to fix the issue

  ## Important Guidelines
  - DO NOT write or suggest code directly
  - Ask clarifying questions about design decisions when needed
  - Consider the project context by reading the [global instructions](../copilot-instructions.md)
  ```

**Step 3 — Create a "Planner" agent with handoff**

- Create the file `.github/agents/planner.agent.md`:
  ```markdown
  ---
  name: Planner
  description: >
    Generates a detailed implementation plan for new features
    or refactoring. Does not modify code directly.
  tools: ['search/codebase', 'fetch']
  model: 'Claude Sonnet 4.6'
  handoffs:
    - label: Implement the plan
      agent: agent
      prompt: Implement the plan outlined above.
      send: false
  ---

  # Planner Instructions

  You are in planning mode. Your task is to generate an implementation plan,
  never to modify code.

  ## Plan Structure
  Every plan must include:
  - **Overview**: brief description of the feature or refactoring task
  - **Files involved**: list of files to create, modify, or delete
  - **Dependencies**: required libraries or modules
  - **Implementation steps**: ordered steps with complexity estimate
  - **Risks and mitigations**: potential problems and how to prevent them
  - **Acceptance criteria**: how to verify the implementation is correct
  ```

**Step 4 — Select and test the agent**

- Open Copilot Chat
- Click the **agent picker** (dropdown at the top of the Chat view)
- Select **Security Reviewer** from the list
- Paste or select a block of code and ask: *"Review this code"*
- Verify that the agent follows the defined report format and does not generate code

**Step 5 — Test the handoff**

- Select the **Planner** agent from the agent picker
- Ask: *"Plan the addition of a JWT authentication system"*
- At the end of the plan, the **"Implement the plan"** button should appear
- Clicking the button switches the conversation to the standard agent with the plan as context


### Success Criteria

- [ ] At least two `.agent.md` files exist in the `.github/agents` folder
- [ ] The custom agents appear in the agent picker of the Chat view
- [ ] The Security Reviewer produces reports without generating code
- [ ] The Planner offers a handoff to the implementation agent

---

## Exercise 6: Hooks — Automation at Agent Lifecycle Points

This exercise teaches you how to configure hooks — shell commands that run automatically in response to specific events during an agent session. Hooks are deterministic: unlike instructions, they execute every time the condition is met.

### Prerequisites

Before you begin, make sure you have:

- Visual Studio Code version 1.109+ (hooks are in Preview)
- Node.js and npm installed
- Prettier installed in the project (`npm install --save-dev prettier`)

### Objectives

1. Create a hook configuration file in JSON format
2. Configure a `PostToolUse` hook to automatically format files after every edit
3. Configure a `PreToolUse` hook to block dangerous commands
4. Verify hook execution in the output channel


**Step 1 — Create the hooks folder**

- Run in the terminal:
  ```bash
  mkdir -p .github/hooks
  ```

**Step 2 — Create an auto-formatting hook**

- Create the file `.github/hooks/auto-format.json`:
  ```json
  {
    "hooks": {
      "PostToolUse": [
        {
          "type": "command",
          "command": "npx prettier --write \"$TOOL_INPUT_FILE_PATH\"",
          "timeout": 15
        }
      ]
    }
  }
  ```
- This hook runs Prettier on every file modified by the agent

**Step 3 — Create a security hook**

- Create the file `.github/hooks/security-gates.json`:
  ```json
  {
    "hooks": {
      "PreToolUse": [
        {
          "type": "command",
          "command": ".github/hooks/scripts/validate-command.sh",
          "timeout": 10
        }
      ]
    }
  }
  ```
- Create the scripts folder and the validation script:
  ```bash
  mkdir -p .github/hooks/scripts
  ```
- Create the file `.github/hooks/scripts/validate-command.sh`:
  ```bash
  #!/bin/bash
  # Read the JSON input from stdin
  INPUT=$(cat)
  TOOL_NAME=$(echo "$INPUT" | jq -r '.toolName // empty')
  COMMAND=$(echo "$INPUT" | jq -r '.toolArgs.command // empty')

  # Block dangerous commands
  if [[ "$TOOL_NAME" == "bash" ]]; then
    if echo "$COMMAND" | grep -qE '(rm\s+-rf\s+/|DROP\s+TABLE|DROP\s+DATABASE)'; then
      echo '{"decision": "deny", "reason": "Dangerous command blocked by security policy"}'
      exit 1
    fi
  fi

  # Approve everything else
  exit 0
  ```
- Make the script executable:
  ```bash
  chmod +x .github/hooks/scripts/validate-command.sh
  ```

**Step 4 — Verify hook execution**

- Open Copilot Chat in agent mode
- Ask the agent to create or modify a TypeScript file
- After the edit, verify that the file was automatically formatted by Prettier
- Open the output channel: **View > Output**, then select **GitHub Copilot Chat Hooks** from the dropdown
- Verify that the log shows the PostToolUse hook execution

**Step 5 — Test command blocking**

- Ask the agent: *"Delete all files in the project root folder"*
- The PreToolUse hook should block the command and return the error message
- Verify in the output channel that the hook intercepted and denied the command


### Success Criteria

- [ ] At least two `.json` files exist in the `.github/hooks` folder
- [ ] After every file edit by the agent, Prettier runs automatically
- [ ] Dangerous commands are blocked by the PreToolUse hook
- [ ] The "GitHub Copilot Chat Hooks" output channel shows execution logs

---

## Exercise 7: MCP — Connecting External APIs and Databases

This exercise teaches you how to configure an MCP (Model Context Protocol) server in VS Code to give Copilot access to external tools such as databases, APIs, and services. MCP is the open standard for connecting AI agents to external data sources.

### Prerequisites

Before you begin, make sure you have:

- Visual Studio Code version 1.100+
- Docker installed (for the PostgreSQL MCP server example) or Node.js
- Basic familiarity with REST APIs

### Objectives

1. Configure an MCP server in the workspace settings
2. Verify that MCP tools are available in Copilot Chat
3. Use an MCP tool to query an external data source


**Step 1 — Understand MCP configuration**

- MCP servers are configured in the `.vscode/settings.json` workspace file or via the Command Palette
- Open the Command Palette (`Ctrl+Shift+P`) and search for **"MCP: Add Server"**
- VS Code will guide you through the configuration

**Step 2 — Configure an example MCP server (filesystem)**

- Open or create the `.vscode/settings.json` file and add the configuration:
  ```json
  {
    "mcp": {
      "servers": {
        "filesystem": {
          "command": "npx",
          "args": [
            "-y",
            "@modelcontextprotocol/server-filesystem",
            "${workspaceFolder}"
          ]
        }
      }
    }
  }
  ```
- This MCP server provides Copilot with tools to read, write, and search files in the filesystem

**Step 3 — Configure a PostgreSQL MCP server (optional)**

- If you have Docker, start a test PostgreSQL container:
  ```bash
  docker run --name pg-demo -e POSTGRES_PASSWORD=demo123 -p 5432:5432 -d postgres:16
  ```
- Add the PostgreSQL MCP server to the configuration:
  ```json
  {
    "mcp": {
      "servers": {
        "postgres": {
          "command": "npx",
          "args": [
            "-y",
            "@modelcontextprotocol/server-postgres",
            "postgresql://postgres:demo123@localhost:5432/postgres"
          ]
        }
      }
    }
  }
  ```

**Step 4 — Verify tool availability**

- Open Copilot Chat in agent mode
- Click the **Tools** icon (gear/wrench icon) in the chat bar
- Verify that the configured MCP server's tools appear in the list
- Tools will be prefixed with the server name (e.g., `filesystem`, `postgres`)

**Step 5 — Use MCP tools in a conversation**

- Ask Copilot: *"List all TypeScript files in the project"*
- Copilot should use the `filesystem` tool to search for files
- If you configured PostgreSQL, ask: *"Show me the tables in the database"*
- Copilot should use the `postgres` tool to execute the query

**Step 6 — Combine MCP with a custom agent (optional)**

- Create an agent that specifically uses MCP tools:
  ```markdown
  ---
  name: Database Admin
  description: Manages and queries the project's PostgreSQL database
  tools: ['postgres', 'search/codebase']
  ---

  You are an expert DBA. You help write queries, optimize schemas,
  and troubleshoot PostgreSQL database performance issues.
  Always use the postgres tool to execute queries.
  ```


### Success Criteria

- [ ] At least one MCP server is configured in `.vscode/settings.json`
- [ ] The MCP server's tools appear in the Copilot Chat tool list
- [ ] Copilot uses MCP tools to answer relevant questions
- [ ] (Optional) A custom agent is configured to work specifically with MCP tools

---

### Useful Resources

- [Official docs: Customize Copilot in VS Code](https://code.visualstudio.com/docs/copilot/customization/overview)
- [Always-on Instructions](https://code.visualstudio.com/docs/copilot/customization/custom-instructions)
- [File-based Instructions](https://code.visualstudio.com/docs/copilot/customization/custom-instructions)
- [Prompt Files](https://code.visualstudio.com/docs/copilot/customization/prompt-files)
- [Agent Skills](https://code.visualstudio.com/docs/copilot/customization/agent-skills)
- [Custom Agents](https://code.visualstudio.com/docs/copilot/customization/custom-agents)
- [Agent Hooks](https://code.visualstudio.com/docs/copilot/customization/hooks)
- [MCP Servers in VS Code](https://code.visualstudio.com/docs/copilot/chat/mcp-servers)
- [Agent Plugins](https://code.visualstudio.com/docs/copilot/customization/agent-plugins)
- [Awesome GitHub Copilot Customizations](https://github.com/github/awesome-copilot)
- [Blog: Custom Instructions to unlock the power of Copilot](https://code.visualstudio.com/blogs/2025/03/26/custom-instructions)
