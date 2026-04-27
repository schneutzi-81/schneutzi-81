# Copilot in the IDE

> Note: For the most basic "What can Copilot do?" scenario, use the `demo-unit-test-coverage` prompt to have Agent Mode
> add some unit tests.

# **About Up the Demo**

- **About the App:** This is a modern TypeScript web-app with separate API and Frontend (React) projects that you will
  enhance with Copilot Agent Mode, Vision, MCP Servers and GHAS/Actions.
- **Why:** Demonstrate how Copilot can analyze and enhance existing code automatically, understand images,
  vulnerabilities and testing and how you can extend Copilot's capabilities with MCP server.
- **Demos**: You don't have to do all these demos, or do them in order. Get comfortable with the scenarios and practice
  them and then mix/match according to your audience.
- **MCP Servers**: The GitHub MCP server runs via Docker. You will need to install Docker locally to run it (it should
  work fine in a Codespace automatically). I use Podman for my Mac. Install this _before_ you attempt this demo! You'll
  also need a PAT that has enough permissions for your demos. Details below.
- **Padawan**: If you want to use Padawan, you have to ensure that it is enabled on the repo, that Actions are enabled
  and that you have a branch protection rule for `main`. I recommend creating a PR required for changes to Default
  branch with 1 required reviewer.
- **Local vs Codespaces:**
    - This demo can work in a Codespace - but some scenarios (like running Playwright tests) require that you work in a
      local VSCode (clone the repo locally)
    - The API endpoint is automatically detected for both browser-based and local VS Code Codespaces. The API port (
        3000) is configured as `public` in the devcontainer and enforced in a post setup command for browser access, and
              the frontend dynamically constructs the correct endpoint URL.
    - Although the API port (3000) is set to `public` automatically, it's recommended to verify the port visibility in
      Codespace before the demo to avoid unexpected CORS errors.
    - Make sure you **PRACTICE** this demo _before_ trying it in front of customers
    - Remember, Copilot is non-deterministic so you can't always predict exact behavior. Make sure you are comfortable
      with this environment so you can pivot quicky!
    - You don't have to use **VS Code Insiders** Version unless you want to demo features that you know are in preview.
        - If you want to access the Insiders Version in the web-version of a Codespace, click on the gear-icon on the
          bottom-left and select `Switch to Insiders Version...`

  ![Switch to Insiders](./images/vscode-switch-to-insiders.png)

## **Building, Running and Debugging the code**

Refer to [the build docs](./../docs/build.md).

# ** Setting up **

Run commands:

- `gh extension install github/gh-aw` to install the GitHub CLI extension for GitHub Actions and MCP server management
- `make install` to install dependencies
- `make dev` to start the API and Frontend in development mode with hot-reloading

---

## General Copilot Features

### Demo: Code Completion and Context Awareness

Show Copilot's ability to understand context and provide relevant code completions.

- **What to show:** Context-aware code completions in both API and Frontend projects.
- **Why:** Demonstrate how Copilot can speed up coding by providing relevant suggestions based on the current file and
  project context.
- **How:**
    1. Create a new branch for the demo: `unsafe`
    2. Open a file in the API project (e.g., `api/repositories/ordersRepo.ts`).
    3. Add a new method to fetch orders by name. Write a comment describing the method, then start typing the method
       signature.
    4. Show how Copilot suggests the complete method implementation based on the comment and existing code patterns.
    5. Inline query parameter to sql statement to demonstrate code reviews.
    6. Open the file in the API project (e.g., `api/routes/product.ts`).
    7. Add a new route to partially update a product. Write a comment describing the route, then start typing the route
       handler.
    8. Show how Copilot suggests the complete route implementation based on the comment and existing code patterns.
    9. Accept the suggestions and explain how Copilot understands the context of the project to provide relevant code.
    10. Commit the changes to the `unsafe` branch.
    11. Push the branch to the remote repository.
    12. Create a Pull Request from the `unsafe` branch to `main` with summary and assign copilot.

### Demo: Enhancing Unit Tests and Coverage

#### Option 1: Using Coding Agent

If you want to demo Copilot Coding Agent, there is an Issue for improving Code Coverage on the repo - it should be Issue
#2 (`Improve test coverage for API` - created as part of the demo spinup). Assign this to Copilot - that's it. This
takes about 15 mins, so do this ahead of time if necessary!

> [!TIP]
> **Using 3rd Party Agents:** When assigning an issue to Copilot, you can open the **Web agents panel** or use the *
*Agent Picker** in VSCode to select an alternative agent such as **Claude** or **Codex** instead of the default Copilot
> Coding Agent. This allows you to leverage different agents for your coding tasks.

#### Option 2: Live Coding

- **What to show:** Copilot generating multiple tests, executing them, analyzing coverage and self-healing, plus
  demonstrate efficient use of custom prompts for testing workflows.
- **Why:** Show Copilot's ability to quickly and easily generate tests, validate them, self-heal and analyze coverage.
  Also demonstrate how custom prompts can standardize testing practices.
- **Approach 1 - Custom Prompt (Recommended for demos):**
    1. Open the [demo-unit-test-coverage.prompt.md](../../.github/prompts/demo-unit-test-coverage.prompt.md) file
    2. Show the prompt structure: pre-configured for Agent mode, comprehensive tool list, detailed testing requirements
    3. Explain how it includes specific coverage requirements, CRUD operations, error handling, etc.
    4. Click "Run" to execute the automated test generation
    5. Show how it creates comprehensive test files for both Product and Supplier routes
    6. Demonstrate the self-healing capabilities when tests fail

- **Approach 2 - Manual Chat (For deeper explanation):**
    1. Ask Copilot to `run tests, analyze coverage and add missing Branch tests to include tests for untested scenarios`
    2. Show Agent working on the tests and adding new tests for the API Branch route
    3. Show Copilot "self-healing" (if tests fail)
    4. Accept the changes
    5. Ask Copilot to `add tests for the Product route` to show generation of new tests

- **Key Takeaway**: Custom prompts can encapsulate testing best practices and ensure comprehensive coverage
  automatically.

#### Option 3: Copilot CLI

Before you start check the issue number for `Improve test coverage for API`.

> [!TIP]
> **Open Copilot CLI in VS Code:** There are several ways to start Copilot CLI. From VS Code you can start by selecting
> New Copilot CLI Session from the New Chat menu in Copilot Chat. You can also open a Terminal in the Editor View using
`Command + Shift + P` and then `Terminal: Create New Terminal in Editor Area` or drag a terminal window into the edit
> area. From VS Code you can run Copilot CLI commands while still having access to your workspace and the ability to
> quickly open files that Copilot CLI generates or modifies or to view the Copilot CLI sessions in Copilot Chat. You can
> hide the Workspace Explorer and Copilot Chat if you want a cleaner workspace.

- **What to show:** Using Copilot CLI to generate tests and analyze coverage directly from the terminal using parallel
  tasks.
- **Why:** Demonstrate that Copilot's capabilities are not limited to the IDE and can be accessed from the command line
  for quick iterations and automation.
- **How:**
    1. In the terminal open copilot with `copilot --yolo`. This auto-approves tool calls and terminal commands. You can
       also go into auto-approve mode using the `/yolo` command.
    2. Type `?` and share a few key commands like cycle modes, model selection, editing, fleet mode, tasks and session
       information.
    3. Shift to plan mode (`Shift + TAB`) and type `Write an implementation plan for #\<issue-number\>`.
    4. Once that is done, select `2. Exit plan mode and I will prompt myself`.
    5. Open the plan and share its contents (`Command + Click` to open). You can see that when you use Copilot CLI in VS
       Code you have easy access to files.
    6. Shift to autopilot (`Shift + TAB`) and type `/fleet implement the plan` Share that `autopilot` mode completes
       tasks without needing your input after the initial instruction and that `/fleet` enables parallel subagent
       execution.
    7. Wait for Copilot to get started on the tasks and then type `/tasks` to show how Copilot runs multiple tasks in
       the background using subagents. Scroll to one of the tasks and then type `Enter` to open the Subagent Details
       view. Go back with `Escape`.
    8. The tasks take some time to complete, so it is best to continue with other demos and then revisit this demo
       later.
    9. Once you revisit, type `/session` to view the session details. If you want to run another task, type `/model` and
       select a different model (for example the latest GPT or Gemini model) and then type `/review` to review the
       changes. Have it fix any suggestions.

- **Key Takeaway**: Copilot CLI provides a powerful way to interact with Copilot's capabilities directly from the
  terminal, enabling quick iterations and parallel task execution. Running the CLI from VS Code allows you to easily
  access generated files and maintain context with your workspace.

### Demo: Using Vision and Agent to Generate Cart Functionality

> [!NOTE]
> **Quick Start Option**: Use the `demo-cart-page.prompt.md` custom prompt for an automated demo. This prompt will have
> Agent Mode implement the complete Cart Page functionality automatically with proper context and tools pre-configured.

- **What to show:** "Vibe coding" using Agent Mode and Vision to complete complex tasks, plus demonstrate custom prompt
  efficiency.
- **Why:** Demonstrate how Copilot Vision can detect design patterns, how Agent can understand a codebase and create
  complex changes over multiple files, and how custom prompts can streamline complex demos.

#### Approach 1 - Custom Prompt (Recommended for demos)

1. Open the [demo-cart-page.prompt.md](../../.github/prompts/demo-cart-page.prompt.md) file
2. Show the prompt structure: comprehensive tool list, detailed context about the current state
3. Attach the [cart image](../docs/design/cart.png) to the prompt
4. Click "Run" to execute the entire cart implementation automatically
5. Show how the custom prompt handles the complete workflow with proper context

#### Approach 2 - Manual Chat (For deeper explanation)

1. Run the App to show the original code. Once the site starts, click on "Products" in the NavBar and show the Product
   Page. Add an item to the Cart - note that nothing actually happens, except a message saying, "Added to Cart". Explain
   that there is no Cart in the frontend app currently.
2. Open Copilot and switch to "Plan" mode.
3. Attach the [cart image](../docs/design/cart.png) using the paperclip icon or drag/drop to add it to the chat.
4. Enter the following prompt:

    ```txt
    I need to implement a simple Cart Page. I also want a Cart icon in the NavBar that shows the number of items in the Cart.
    ```

5. Highlight that Copilot has suggested changes and planned the components to add/modify.
6. (OPTIONAL if you have the GitHub MCP Server configured): Ask Copilot to
   `create an issue in my repo to implement the Cart page and Cart icon`
7. Show the issue in the repo
8. Switch to "Agent" mode in Copilot Chat. Switch to `Claude 3.5 Sonnet` (a good implementation model) and enter this
   prompt:

    ```txt
    Implement the changes.
    ```

9. Show Copilot's changes and how you can see each one and Keep/reject each one.
10. Accept Copilot's suggested fixes.
11. Go back to the Frontend app. Navigate to Products. Show adding items to the cart (note the icon updating). Click on
    the Cart icon to navigate to the Cart page. Show the total, and adding/removing items from the cart.

- **Key Takeaway**: Custom prompts provide consistency and can encapsulate complex workflows that would otherwise
  require multiple manual steps.

## Customizations

### Demo: Custom Instructions and Repository Configuration

- **What to show:** Copilot's **Custom Instructions** feature using the existing `.github/copilot-instructions.md`
  configuration.
- **Why:** Demonstrate that Copilot can be customized and personalized for internal libraries, coding standards, and
  team practices that don't exist in the foundational models.
- **How:**
    1. Show the existing [.github/copilot-instructions.md](../../.github/copilot-instructions.md) file in the repository
    2. Explain how this file provides context about:
        - Repository information (owner, repo name)
        - Architecture references
        - Build and testing instructions
    3. **Demo Enhanced Custom Instructions**:
        1. Option 1: Apply the Patch Set `Copilot: Custom Instructions`[^1] which will update the custom-insturctions
           file
        2. Option 2: Update the custom instructions file by hand, adding these additional guidelines

            ```markdown
            ## Additional Guidelines for REST APIs
            
            For REST APIs, use the following guidelines:
            
            * Use descriptive naming
            * Add Swagger docs for all API methods
            * Implement logging and monitoring using [TAO](../docs/tao.md)
              - assume TAO is installed and never add the package
            ```

    4. Show the [TAO](./tao.md) documentation to demonstrate the fictional internal library
    5. Ask Copilot to `add observability to the Supplier route using our internal standards`
    6. Show how Copilot uses the custom instructions to implement TAO observability patterns
    7. **Note**: Explain that this will not compile since TAO doesn't really exist - this demonstrates how custom
       instructions can reference internal frameworks
    8. **Key Takeaway**: Custom instructions allow teams to encode their specific practices, internal libraries, and
       coding standards

## TDD Agent Mode

### Demo: Custom Agents Driving a Multi-Phase Workflow

- **What to show:** Three custom agents (`tdd-red`, `tdd-green`, `tdd-blue`) that each own one phase of a TDD workflow,
  demonstrating how custom agents can enforce role-specific constraints and hand off context between phases.
- **Why:** TDD is a familiar, well-structured process that makes it easy to show how custom agents can be scoped to a
  single responsibility, how the developer orchestrates handoffs between them, and how each agent's prompt constrains
  what it will and won't do.


- **How:**

    1. **Plan the Feature (Plan Mode):**
        - In VSCode, open Copilot Chat and switch to **Plan** mode (or use `/plan` in the CLI)
        - Enter the prompt: `I want a new method that gets the orders for a branch ID`
        - Walk through the planning conversation — Copilot will ask clarifying questions and produce a structured plan
        - Once satisfied, keep the plan open — it serves as input for the agents in the next phases

    2. **🔴 Red Phase — `tdd-red` Agent:**
        - Switch to the `tdd-red` agent from the agents dropdown
        - Prompt it with a single requirement from the plan, e.g.:
          `Write a failing test for getting orders by branch ID`
        - Show how the agent is **constrained by its prompt** — it writes a test but refuses to write implementation
          code
        - Show the test output confirming the test fails (e.g., HTTP 404 — route not implemented)

    3. **🟢 Green Phase — `tdd-green` Agent:**
        - Switch to the `tdd-green` agent
        - Prompt: `Make the failing test pass`
        - Show how this agent's scope is different — it reads existing tests and writes only enough implementation to
          make them pass
        - Show the test passing ✅

    4. **🔵 Blue Phase — `tdd-blue` Agent:**
        - Switch to the `tdd-blue` agent
        - Prompt: `Refactor the implementation while keeping tests green`
        - Show how this agent focuses on code quality — refactoring, naming, documentation — while continuously running
          tests to prevent regressions

    5. **Repeat the Cycle:**
        - Go back to `tdd-red` for the next requirement from the plan
        - Cycle through Red → Green → Blue to show how agents hand off context through the code itself (tests,
          implementation, plan)

    6. **Key Points to Highlight:**
        - **Role-Scoped Agents**: Each agent has strict boundaries — Red won't implement, Green won't refactor, Blue
          won't add features
        - **Developer-Driven Orchestration**: The developer decides when to switch agents — there's no magic; the
          workflow is transparent
        - **Context Through Code**: Agents hand off context via the artifacts they produce (tests, implementation, plan
          doc) — not hidden state
        - **Prompt Engineering in Practice**: Open `.github/agents/` to show how each agent's prompt enforces its role
          and constraints
        - **Reusable Pattern**: This same multi-agent pattern applies to any phased workflow (e.g., design → implement →
          review)

    7. **(Optional) Show the Agent Configuration:**
        - Open `.github/agents/` to show the three TDD agents and their prompt files:
            - `tdd-red.agent.md`: Scoped to writing failing tests only
            - `tdd-green.agent.md`: Scoped to minimal implementation
            - `tdd-blue.agent.md`: Scoped to refactoring with tests green
        - Highlight how each agent's system prompt is what enforces the constraints you just saw in action

- **Key Takeaway**: Custom agents let teams encode multi-phase workflows where each agent owns a clear responsibility.
  The TDD cycle is a great example — but the pattern generalizes to any process where you want role-specific AI behavior
  with transparent handoffs.

# GitHub Copilot Skills Demo

This walkthrough demonstrates how to use **Agent Skills** to generate high-quality, consistent code that follows
specific instructions, formats or tools.

## What are Skills?

Agent Skills are folders of instructions, scripts, and resources that GitHub Copilot can load when relevant to perform
specialized tasks. Skills are an [open standard](https://agentskills.io/) that works across multiple AI agents,
including GitHub Copilot in VS Code, GitHub Copilot CLI, and GitHub Copilot coding agent.

**Key benefits of Agent Skills:**

- **Specialize Copilot**: Tailor capabilities for domain-specific tasks without repeating context
- **Reduce repetition**: Create once, use automatically across all conversations
- **Compose capabilities**: Combine multiple skills to build complex workflows
- **Efficient loading**: Only relevant content loads into context when needed
- **Portable**: Works across VS Code, Copilot CLI, and Copilot coding agent

### Agent Skills vs Custom Instructions

| Aspect          | Agent Skills                                                | Custom Instructions                    |
|-----------------|-------------------------------------------------------------|----------------------------------------|
| **Purpose**     | Teach specialized capabilities and workflows                | Define coding standards and guidelines |
| **Portability** | Works across VS Code, Copilot CLI, and Copilot coding agent | VS Code and GitHub.com only            |
| **Content**     | Instructions, scripts, examples, and resources              | Instructions only                      |
| **Scope**       | Task-specific, loaded on-demand                             | Always applied (or via glob patterns)  |
| **Standard**    | Open standard ([agentskills.io](https://agentskills.io/))   | VS Code-specific                       |

**Use Agent Skills when you want to:**

- Create reusable capabilities that work across different AI tools
- Include scripts, examples, or other resources alongside instructions
- Define specialized workflows like API generation, testing, or deployment processes

**Use Custom Instructions when you want to:**

- Define project-specific coding standards
- Set language or framework conventions
- Apply rules based on file types using glob patterns

### Skill Structure

Skills are defined in `.github/skills/` or `.claude/skills/` directories and contain:

- `SKILL.md` - The skill definition with YAML frontmatter (name, description) and detailed instructions
- Additional resources - Scripts, examples, templates, and reference documentation

---

## Demo: Using the `api-endpoint` Skill to Add a New Entity

### Why

- **Consistency**: New developers (or AI assistants) need to have specialized knowledge to produce high quality results.
  Skills encode institutional knowledge so that code generation is consistent and bespoke.
- **Reduced Review Overhead**: When code generation follows established patterns, reviewers can focus on business logic
  rather than style/convention fixes.
- **Faster Onboarding**: New team members can use Skills to understand how things are done in your codebase.
- **Scalability**: As codebase grows, Skills ensure consistency, producing higher quality results and making the
  codebase easier to maintain.

### What to Show

1. **The Skill Definition**: Show the `api-endpoint` skill structure and explain how it encodes a specific technical
   skill (in this case, creating an API endpoint)
2. **Natural Language Prompt**: Demonstrate using a simple prompt to generate a complete, production-ready API endpoint
3. **Generated Artifacts**: Show all the files Copilot creates (model, repository, routes, migration, seed data, tests)
4. **Pattern Adherence**: Highlight how the generated code follows the exact same patterns as existing code

### How

#### Part 1: Explore the Skill

0. Enable the `chat.useAgentSkills` setting in VSCode to use Agent Skills.
1. Open the [.github/skills/api-endpoint/SKILL.md](../../.github/skills/api-endpoint/SKILL.md) file
2. Walk through the key sections:
    - **Architecture Overview**: Shows the layered architecture (Routes → Repository → Database)
    - **When to Use This Skill**: Trigger conditions for the skill
    - **Workflow Steps**: Step-by-step guide for creating models, repositories, routes, migrations, and seed data
    - **Patterns and Examples**: Concrete code patterns for each component
3. (Optional) Show
   the [references/database-conventions.md](../../.github/skills/api-endpoint/references/database-conventions.md) file
   to demonstrate how supporting documentation is included

#### Part 2: Generate the DeliveryVehicle Entity

1. Open Copilot Chat and switch to **Agent** mode
2. Enter the following prompt:

   ```txt
   Add a new API endpoint for a new Entity called 'DeliveryVehicle'. Vehicles belong to branches.
   ```

3. Watch as Copilot:
    - Analyzes the existing codebase structure
    - References the `api-endpoint` skill automatically
    - Generates all required components following the established patterns:
        - **Model**: Generates the model using conventions
        - **Repository**: Generates the Repository with CRUD operations
        - **Routes**: Generates the route with full REST endpoints
        - **Migration**: Creates db migrations
        - **Seed Data**: Creates seed data
        - **Tests**: Creates and runs unit tests for the new endpoint

4. Review the generated code and highlight:
    - **Naming Conventions**: Follows naming conventions for entities/methods
    - **Foreign Key Relationship**: The `branchId` field linking to the `branches` table
    - **API Documentation**: Complete OpenAPI annotations for all endpoints
    - **Error Handling**: Consistent use of custom errors
    - **SQL Utilities**: Using specified utils
    - **Unit Tests**: Created and verified unit tests

#### Part 3: Verify the Implementation

1. Accept the changes
2. Run build/unit tests
3. Open the Swagger UI at `http://localhost:3000/api-docs` and show the new DeliveryVehicle endpoints
4. (Optional) Test the CRUD operations using the Swagger UI

---

## Key Takeaways

| Benefit                        | Description                                                                                                    |
|--------------------------------|----------------------------------------------------------------------------------------------------------------|
| **Institutional Knowledge**    | Skills encode your team's patterns and conventions, making them accessible to all developers and AI assistants |
| **Consistent Code Generation** | Every generated endpoint follows the same structure, reducing code review overhead                             |
| **Self-Documenting**           | Skills serve as living documentation of your project's architecture and patterns                               |
| **Scalable Development**       | As your API grows, Skills ensure consistency across all endpoints                                              |
| **Faster Development**         | Developers can generate production-ready code with a simple natural language prompt                            |

---

# Ralph Loop Demo

A "Ralph loop" is an iterative development pattern that uses AI to continuously work on a task until it is completed.
Iterations are fresh to keep from poluting the context window and give the impression of "infinite context".

You can get the same effect as a Ralph loop using custom agents and subagents!

**Key benefits of "Ralph Loop" in VSCode:**

- no need to set up offline agents or compute to run them
- you are still in complete control and can stop the loops any time you want to
- you can continue in the background or hand off to Cloud if you need to
- Copilot is unlikely to create a new AI religion (unless you tell it to :-) )

## Demo: Using the `api-endpoint` Skill to Add a New Entity

In this scenario, we are going to use a "API Test Coverage" loop.
The [looper agent](.github/agents/api-coverage-looper.agent.md) will search the API routes and determine which routes
have tests and which don't. It will then use the `subAgents` tool to invoke
the [test-writer agent](.github/agents/api-test-writer.agent.md) in parallel, (each as a subagent to keep context tight)
to write tests for the route, using the `argument-hint` to pass in the route to write tests for.

Once the loop starts, you can then start a new session and the looper will continue in the background until all the
routes have tests!

### How

1. Open a new Copilot Chat session.
2. Select "Local" and then `API Coverage Looper` as the agent. Select a model - a small model like Haiku or Gemini Flash
   should work just fine for this demo.
3. Type `complete test coverage` in the Chat and let it go.
4. Let it go for a few seconds and point out that a set of parallel subagents has started working each with their own
   thread to keep context clean.
5. You'll have to click "Allow tools" for each subagent (the easiest is to "Allow all tools in this session") otherwise
   every tool invocation will prompt for permission.
6. Point out that this is `yolo` and that you should be careful doing this!
7. At this point you can click the top-left "Back" arrow to start a new Chat - this work continues on the original
   session which you can switch back to in the session view.
8. Once the subagents complete, you should see a final report showing coverage for all the API routes.
