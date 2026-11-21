# Project layout

This repository develops client-specific meta-prompting skills.

- `README.md`: shared introduction and links to client guides.
- `docs/codex.md`, `docs/opencode.md`, and `docs/claude.md`: client installation, usage, and development guides.
- `skills/meta-prompting-codex/`, `skills/meta-prompting-opencode/`, and `skills/meta-prompting-claude/`: independently installable instructions, licenses, and references.
- `skills/meta-prompting-codex/agents/openai.yaml`: Codex display and invocation metadata.
- `.agents/skills/meta-prompting-codex`: relative symlink for project-local Codex discovery.
- `.claude/skills/meta-prompting-claude`: relative symlink for project-local Claude Code discovery.

# Development

Keep each package self-contained and specific to its host. Preserve freely composed prompts and bounded inquiries. Codex queues then ends the turn; OpenCode's calling agent manages synchronous submissions and continuation; Claude Code schedules a one-shot task with CronCreate then ends the turn. Skills share the repository version. Track the goal, pending prompt, and allowance in the conversation.

Validate all packages and documentation links after edits. Keep prose paragraphs unwrapped; preserve line breaks needed by code, tables, and lists. For delivery changes, use a bounded live check when needed and distinguish acceptance from observed arrival. Do not invoke a separate model as a substitute for testing the active client's workflow.

Preserve existing research records unless the user requests their removal.
