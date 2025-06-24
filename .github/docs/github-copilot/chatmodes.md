# Custom chat modes

Note
Custom chat modes are available as of VS Code release 1.101 and are currently in preview.

The built-in chat modes provide general-purpose configurations for chat in VS Code. For a more tailored chat experience, you can create your own chat modes.

Custom chat modes consist of a set of instructions and tools that are applied when you switch to that mode. For example, a "Plan" chat mode could include instructions for generating an implementation plan and only use read-only tools. By creating a custom chat mode, you can quickly switch to that specific configuration without having to manually select relevant tools and instructions each time.

Custom chat modes are defined in a .chatmode.md Markdown file, and can be stored in your workspace for others to use, or in your user profile, where you can reuse them across different workspaces.

You can reference instructions files and tools (sets) in your custom chat mode file.

## Chat mode file structure

A chat mode file is a Markdown file with the .chatmode.md suffix. It has the following two main sections:

- Front Matter metadata header

  - description: A brief description of the chat mode. This description is displayed when you hover the chat mode in the chat mode dropdown list in the Chat view.

  - tools: A list of tool or tool set names that are available for this chat mode. This can include built-in tools, tool sets, MCP tools, or tools contributed by extensions. Use the Configure Tools action to select the tools from the list of available tools in your workspace.

- Body with chat mode instructions

This is where you provide specific prompts, guidelines, or any other relevant information that you want the AI to follow when in this chat mode. You can also reference instructions files by using Markdown links. The chat mode instructions will complement whatever is specified in the chat prompt.

## Chat mode file example

The following code snippet shows an example of a "Plan" chat mode file that generates an implementation plan and doesn't make any code edits.

```markdown
---
description: Generate an implementation plan for new features or refactoring existing code.
tools: ['codebase', 'fetch', 'findTestFiles', 'githubRepo', 'search', 'usages']
---
# Planning mode instructions
You are in planning mode. Your task is to generate an implementation plan for a new feature or for refactoring existing code.
Don't make any code edits, just generate a plan.

The plan consists of a Markdown document that describes the implementation plan, including the following sections:

* Overview: A brief description of the feature or refactoring task.
* Requirements: A list of requirements for the feature or refactoring task.
* Implementation Steps: A detailed list of steps to implement the feature or refactoring task.
* Testing: A list of tests that need to be implemented to verify the feature or refactoring task.
```
