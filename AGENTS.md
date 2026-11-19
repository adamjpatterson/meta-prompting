# Project layout

This repository develops the `meta-prompting` skill.

- `skills/meta-prompting/`: self-contained instructions and human-facing README.
- `skills/meta-prompting/agents/openai.yaml`: display and invocation metadata.
- `skills/meta-prompting/references/`: supporting examples.
- `.agents/skills/meta-prompting`: relative symlink for project-local discovery.

# Development

Keep the skill portable and allow bounded inquiries when useful to the current
task. The assistant composes prompts; `codex queue` delivers them to the same
conversation. Keep runtime state in the conversation rather than creating
mandatory project-local records. Turn limits are behavioral instructions, not
script-enforced guarantees.

Validate metadata and links after edits. For delivery changes, use a bounded live
check when needed and distinguish queue acceptance from observed arrival. Do not
invoke a separate model as a substitute for testing this assistant's workflow.
There is no evidence yet of a general performance advantage.

Earlier implementations remain in Git history. Preserve existing ignored research
records unless the user requests their removal.
