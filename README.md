# Meta-prompting

A skill for constructing useful context through self-generated prompts in the
active conversation. The assistant chooses each next prompt and calls
`codex queue` directly to deliver its exact text as user input to the same thread.
No Python relay or project-local run files are required.

Read the [skill README](skills/meta-prompting/README.md) for how it works and its
limitations. [SKILL.md](skills/meta-prompting/SKILL.md) contains the operating
instructions.

## Use

The assistant may use the skill when context shaping would help the current task.
You can also request it directly:

> Use $meta-prompting to develop an approach to [your problem]. Allow up to four
> prompts and stop when there is a concrete next action.

The host must provide a shell tool, `codex queue`, and `CODEX_THREAD_ID` for the
active conversation. The skill uses the existing Codex session; it needs no
separate model or API key.

The project registers the skill through `.agents/skills/meta-prompting`, a relative
symlink to `skills/meta-prompting`. Automatic skill selection is enabled. If your
client does not discover it, ask the assistant to read the SKILL.md directly.

For installation in another project, see the [installation instructions](skills/meta-prompting/README.md#install-in-another-project).
Tested compatibility and limitations are recorded in the [validation notes](skills/meta-prompting/references/validation.md).

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

Validate skill metadata with the skill-creator validator available in your Codex
installation. Evaluate delivery in a bounded live inquiry: distinguish queue
acceptance from actual arrival, and check that the assistant stops at the stated
allowance. There is no relay test suite or script-enforced turn counter.

## License

[MIT](LICENSE).
