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

## Compatibility and validation

A reported one-prompt check used **OpenCode 1.19.6** on Linux with Bash and an active OpenCode session. See the [validation notes](validation/opencode.md) for its results and the checks still pending for this package.

## Troubleshooting

If the assistant cannot identify your active session, ask it to explain the ambiguity so you can clarify which conversation to use. The skill requires it to confirm the session against the conversation history before submitting a prompt.

If a call hangs or times out, the prompt may already have arrived. Ask the assistant to check the session history before trying again to avoid submitting the same prompt twice.

## Development

The skill follows the [Agent Skills specification](https://agentskills.io/specification). Its source is in `skills/meta-prompting-opencode/`.

To change how the skill works, edit [SKILL.md](../skills/meta-prompting-opencode/SKILL.md) and update this guide to match. That file contains the instructions OpenCode follows when using the skill.

After editing, check that documentation links resolve and validate the package with the [Agent Skills reference validator](https://github.com/agentskills/agentskills/tree/main/skills-ref). Install it in a separate development environment using its installation instructions, then run from this repository's root:

```bash
skills-ref validate skills/meta-prompting-opencode
```

The validator checks package metadata; it does not test prompt delivery or behavior.

For workflow changes, ask OpenCode to run a short inquiry with a small prompt allowance. Inspect the session history and command results: each prompt should receive one reply, and the assistant that started the inquiry should choose any follow-up after the command returns. Check that follow-ups build on previous responses and that the inquiry stops at its goal or prompt allowance. Record your environment and observations in the [validation notes](validation/opencode.md).
