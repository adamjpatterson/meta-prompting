# Meta-prompting

A skill for constructing useful context through self-generated prompts in the
active conversation. The assistant chooses each next prompt and calls
`codex queue` directly to deliver its exact text as user input to the same thread.

Read the [skill README](skills/meta-prompting/README.md) for how it works and how
to install it. [SKILL.md](skills/meta-prompting/SKILL.md) contains the operating
instructions.

## Installation

### User-account installation

Open Codex, then enter this in the Codex chat box:

```text
Use $skill-installer to install https://github.com/adamjpatterson/meta-prompting/tree/main/skills/meta-prompting
```

### Project-local installation

Open Codex in the destination project, then enter this in the Codex chat box:

```text
Use $skill-installer to install https://github.com/adamjpatterson/meta-prompting/tree/main/skills/meta-prompting into this project's .agents/skills directory using --dest.
```

### Troubleshooting

If the skill does not appear in `/skills`, restart Codex. See
[installation and updating](skills/meta-prompting/README.md#install) for both
scopes, terminal commands, and manual copying.

## Use

The skill instructs the assistant to initiate an inquiry when work repeats without
progress, relies on an untested premise, accumulates complexity, or drifts from
the goal. It can reconsider the approach while preserving your objective.
You can also request it directly:

> Use $meta-prompting to develop an approach to [your problem]. Allow up to four
> prompts and stop when there is a concrete next action.

The request has four parts:

| Part              | Example                                     | Purpose                                                                    |
| ----------------- | ------------------------------------------- | -------------------------------------------------------------------------- |
| Skill instruction | `Use $meta-prompting`                       | Selects the skill                                                          |
| Problem statement | `develop an approach to [your problem]`     | Gives the inquiry a goal; replace the placeholder with your actual problem |
| Prompt allowance  | `Allow up to four prompts`                  | Sets the maximum number of self-generated prompts, including the first     |
| Stop condition    | `stop when there is a concrete next action` | Describes when the inquiry has accomplished enough                         |

The allowance is a ceiling, not a target. The assistant can stop earlier when the
condition is met. You can omit the allowance and stop condition and let the
assistant choose them from the task. Each queued prompt starts a subsequent
conversation turn; the allowance counts those prompts, not tool calls.

## Requirements

The host must provide a shell tool, `codex queue`, and `CODEX_THREAD_ID` for the
active conversation. The skill uses the existing Codex session; it needs no
separate model or API key.

`codex queue` also needs access to the local Codex app server. With restricted
access, starting or contacting that server may require approval. Full access
removes the sandbox restriction, but is not required to use the skill. See
[execution permissions](skills/meta-prompting/README.md#execution-permissions).

## Discovery in this repository

The source package is `skills/meta-prompting/`. Codex discovers project skills
under `.agents/skills/`, so `.agents/skills/meta-prompting` is a symbolic link to
the source folder. This lets Codex read the files being edited without keeping
a second copy. Automatic selection is enabled in `agents/openai.yaml`.

The [Agent Skills specification](https://agentskills.io/specification) defines
the package format; [Codex's skill guide](https://learn.chatgpt.com/docs/build-skills#where-to-save-skills)
defines discovery locations. The outer `skills/` folder is our repository
organization, not a required part of an installed skill.

Development checks are recorded in the [validation notes](skills/meta-prompting/references/validation.md).

## Layout

```text
skills/meta-prompting/
  SKILL.md                  Operating instructions
  README.md                 Human-facing explanation
  LICENSE                   Standalone package license
  references/example.md     Illustrative inquiry
  references/validation.md  Validation scope and results
  agents/openai.yaml        Codex metadata and invocation policy
.agents/skills/             Project-local discovery link
```

## Development

The skill's behavior is defined in `skills/meta-prompting/SKILL.md`. Keep that
file and the human-facing README consistent when changing the workflow.

After editing, check that documentation links resolve and that the skill's name
and description remain valid. If your Codex installation includes the
skill-creator validator, use it to check the package metadata.

For workflow changes, try a short inquiry in an active Codex conversation. Verify
that each queued prompt arrives in the same conversation, that the next prompt
builds on the preceding response, and that the inquiry stops at its goal or
prompt allowance. Record the environment and observations in the
[validation notes](skills/meta-prompting/references/validation.md).

## License

[MIT](LICENSE).
