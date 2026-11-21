# Project layout

This repository develops client-specific meta-prompting skills.

- `README.md`: shared introduction and links to client guides.
- `docs/codex.md` and `docs/opencode.md`: client installation, usage, and development guides.
- `skills/meta-prompting-codex/` and `skills/meta-prompting-opencode/`: independently installable instructions, licenses, and references.
- `skills/meta-prompting-codex/agents/openai.yaml`: Codex display and invocation metadata.
- `.agents/skills/meta-prompting-codex`: relative symlink for project-local Codex discovery.

# Development

Keep each package self-contained and specific to its host. Preserve freely composed prompts and bounded inquiries. Codex queues then ends the turn; OpenCode's calling agent manages synchronous submissions and continuation. Track the goal, pending prompt, and allowance in the conversation.

Validate both packages and documentation links after edits. Keep prose paragraphs unwrapped; preserve line breaks needed by code, tables, and lists. For delivery changes, use a bounded live check when needed and distinguish acceptance from observed arrival. Do not invoke a separate model as a substitute for testing the active client's workflow.

Preserve existing research records unless the user requests their removal.
