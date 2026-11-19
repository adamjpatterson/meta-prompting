# Project layout

This repository develops the `meta-prompting` skill.

- `README.md`: human-facing installation, usage, and development guide.
- `skills/meta-prompting/`: self-contained skill instructions and license.
- `skills/meta-prompting/agents/openai.yaml`: display and invocation metadata.
- `skills/meta-prompting/references/`: supporting examples.
- `.agents/skills/meta-prompting`: relative symlink for project-local discovery.

# Development

Keep the skill portable and allow bounded inquiries when useful to the current
task. The assistant composes prompts; `codex queue` delivers them to the same
conversation. The assistant tracks the goal, pending prompt, and allowance in
the conversation.

Validate metadata and links after edits. For delivery changes, use a bounded live
check when needed and distinguish queue acceptance from observed arrival. Do not
invoke a separate model as a substitute for testing this assistant's workflow.

Preserve existing research records unless the user requests their removal.
