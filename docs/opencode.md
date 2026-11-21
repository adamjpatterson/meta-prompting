# Meta-prompting for OpenCode

This package uses `opencode run -s` to submit a self-generated prompt to the active session. The call waits for a response; the calling agent then decides whether to continue.

## Installation

Check out the `main` branch of [this repository](https://github.com/adamjpatterson/meta-prompting/tree/main). Copy the entire `skills/meta-prompting-opencode/` folder, including its license and references, into one of these locations:

- Project: `.opencode/skills/meta-prompting-opencode/`.
- User account: `~/.config/opencode/skills/meta-prompting-opencode/`.

These are OpenCode's native [skill discovery locations](https://opencode.ai/docs/skills/). Use one installation scope. Ask the agent to load `meta-prompting-opencode`; if it cannot discover it, ask it to read the installed `SKILL.md` directly.

If you installed the earlier combined `meta-prompting` skill, preserve local edits and replace that copy with this package to avoid duplicate entries. Do not use the Codex package for this session.

## Updating

Replace the installed folder with the current package from the same branch after preserving local edits. Ask the agent to reread its `SKILL.md`. Keep the installation in its existing scope.

## Usage

> Use the meta-prompting-opencode skill to develop an approach to [your problem]. Allow up to four prompts and stop when there is a concrete next action.

The request selects the skill, states the problem, sets a maximum prompt count, and supplies a stopping condition. The allowance is a ceiling, not a target. You can omit the allowance and stopping condition and let the agent choose when enough has been clarified.

For a first task:

> Use meta-prompting-opencode to review this project's README from a newcomer's perspective. Allow one prompt. Present the most useful onboarding improvement before editing.

## How it works

The calling agent verifies the current session using `opencode session list` and matching conversation history from `opencode export <session-id>`. It submits one safely quoted prompt using:

```bash
opencode run -s '<active-session-id>' 'Your prompt text'
```

The receiving execution answers without submitting another prompt. Once the call returns, the caller uses the response to decide whether to continue. It counts each submission once, keeps one prompt pending, and stops when the inquiry is sufficient or the allowance is exhausted.

## Requirements

Requires OpenCode, shell access, the `opencode` executable, and a verified active session ID. The client must permit the command to access its session storage and execute the response.

## Troubleshooting

If session identification is ambiguous, resolve it before submission. A title or most-recent session alone is not sufficient evidence. If a call hangs or times out, do not automatically retry: the prompt may already have been admitted. Inspect session history before deciding how to recover.

## Validation and development

A reported OpenCode 1.19.6 check observed a prompt and response once in the same session and stopping after one prompt. See the [validation record](../skills/meta-prompting-opencode/references/validation.md) for scope and remaining unknowns.

The operating instructions are in [SKILL.md](../skills/meta-prompting-opencode/SKILL.md). Keep this guide consistent with them. Install the [Agent Skills reference validator](https://github.com/agentskills/agentskills/tree/main/skills-ref) in a separate development environment, then run from the repository root:

```bash
skills-ref validate skills/meta-prompting-opencode
```

Check links and use a bounded live inquiry for delivery changes. Record observed arrival, response, continuation, and stopping rather than inferring success from command acceptance alone.
