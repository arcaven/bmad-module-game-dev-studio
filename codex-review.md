# Editing Code Review to use Codex from Claude Code

If you're tired of opening another terminal session just to get Codex to do an adversarial code review of your stories in Claude Code, this is for you.

This walkthrough assumes that you are:
* Using Anthropic's Claude Code with Opus or Sonnet
* Have OpenAI's Codex installed as an MCP with Codex. This allows Claude Code to send commands directly to Codex without leaving Claude Code.
* Have the BMB (BMad Builder) module installed.

## Installing Codex as an MCP

If you need to install Codex as an MCP, type the following command from your shell:
``` 
claude mcp add codex -- codex -m gpt-5.2-codex -c model_reasoning_effort=high mcp-server
```

You can then confirm it loads successfully via the command:

```
claude mcp list
```

### Converting the code-review workflow to v6

Currently the code-review workflow is still in legacy v4 format. We'll need to convert it first so BMB can edit it.

Run the BMB Workflow agent:

```
/bmad-bmb-workflow-builder.agent
```

Type RW to rework workflow to v6 format. When asked, supply the code review workflow

```
@_bmad/bmm/workflows/4-implementation/code-review/
```

For BMGD, you'll want to use the GDS address instead.

```
@_bmad/gds/workflows/4-implementation/code-review/
```

The BMB agent will analyze the workflow, then print out a detailed summary asking what you want to change. We don't want to change anything yet, so just respond "Workflow is fine, just convert it." The agent will then report what work it is proposing to do for each step of the workflow process - at each prompt enter C to continue. The workflow will then be converted. There will be quite a few steps - this is not a trivial conversion process. (Eventually BMB will figure out you've been approving every step and stop asking!) Finally, when it's done, enter C to complete the build, and S to skip validation (you're about to edit this again, so no need.)

As always, clear context between major steps, and then invoke the BMB agent again:

```
/bmad-bmb-workflow-builder.agent
```

This time, enter EW to edit an existing workflow. When prompted, enter the path to the workflow you just saved. Most likely, it's here:

```
@_bmad_output/bmb-creations/workflows/code-review/workflow.md
```

The agent will complain the workflow's not validated. We know it's fine since the only changes we made were structural, so we'll enter S to skip validation again, then C to Continue to Discovery. Now, at the menu, enter O for other, and then we'll describe what we want to do in detail.

```
I want this code review workflow to have code be reviewed by the Codex MCP. Once Codex analyzes and reviews the code, then reformat its recommendations into the proper format and continue with the code review process.
```

This is how the agent responded -- yours may be slightly different.

```
Interesting - integrating Codex MCP as the review engine. Let me ask a few clarifying questions to make sure I scope this correctly:

  1. Codex Placement - Looking at the current flow (init → attack plan → execute review → present findings → update status), should Codex replace the execute-review step entirely, or should it be an additional step that feeds into the existing review
  execution?
  2. Codex Input - What should be sent to Codex for analysis? The story file's changed files? A diff? The full source files identified in the attack plan?
  3. Reformatting - When you say "reformat its recommendations into the proper format," do you mean mapping Codex findings into the existing checklist/backlog format the workflow already uses? Or a new format?
  4. Fallback - If Codex is unavailable or returns an error, should the workflow fall back to the agent-driven review, or halt and notify the user?
```

And my answers:

```
  1. Codex should replace the execute-review step entirely. We want to use Codex's LLM for the actual code review.
  2. The full source files identified in the attack plan.
  3. Map Codex findings into the existing checklist/backlog format.
  4. The workflow should halt, notify the user, and then optionally continue.
```

If the BMB agent understood what you wanted (it'll rephrase and repeat it back to you) enter C to continue. If you have any changes, enter M to add them.

The BMB agent will then proceed to edit, if all went well, only one of the workflow files, step-03-execute-review.md. It will map out the changes, tell you what they are, and wait for your approval. If they look like this, go ahead and approve. (Note that in my example I am editing the workflow for BMGD, thus the game-specific references.)

```
Current state of: step-03-execute-review.md

  The step currently has the agent itself execute a 4-part adversarial review:
  1. Git vs Story Discrepancy Review
  2. AC Validation
  3. Task Completion Audit
  4. Code Quality Deep Dive
  5. Enforce minimum 3 issues
  6. Auto-proceed to step 4

  All review logic runs inside the agent's own context.

  ---
  Proposed modification:

  Replace the review execution body so that:

  1. Gather Files - Collect the full source files identified in the attack plan (from step 2)
  2. Send to Codex MCP - Submit files to Codex with a structured review prompt covering all 4 review areas (discrepancies, AC validation, task audit, code quality + game-specific checks)
  3. Error Handling - If Codex is unavailable or errors:
    - Halt the workflow
    - Notify the user with the error details
    - Present menu: [R]etry Codex / [M]anual Review (fallback to agent-driven) / [X] Exit
  4. Reformat Findings - Map Codex response into the existing severity-categorized findings format (CRITICAL/HIGH/MEDIUM/LOW with file:line evidence)
  5. Enforce Minimum - Same 3-issue minimum enforcement
  6. Auto-Proceed - Same auto-proceed to step 4

  The step retains the same frontmatter, step goal structure, and success/failure metrics format. The nextStepFile reference stays unchanged.

  Should I apply this change?
```

Once approved, BMB will edit the workflow, and ask you what to do next. NOW it's time to run validation - select V to validate. In my case, validation turned up a few minor warnings that the first dev pass missed - you can enter F to fix them, or just enter C to complete if you think they're minor. After the fixes are complete, enter F to finish the edit session.

One last step remains -  a way to use this new workflow. For Claude Code, there needs to be a pointer in .claude/commands to trigger workflows. Luckily, you can just have Claude Code create it:

```
Create a markdown file in @.claude/commands that launches the new code review workflow you just created. Name the markdown file bmad-codex-review.md
```

Once that's done, you can now enter

```
/bmad-codex-review 1.6
```

whenever you would otherwise do /bmad-bmm-code-review 1.6.

Enjoy the best of both worlds - Claude Code as your one client, using Opus/Sonnet as your code development agents and GPT-Codex as your adversarial code reviewer!
