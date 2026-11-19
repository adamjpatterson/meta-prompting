# Meta-prompting

Meta-prompting lets the assistant construct context for its own subsequent work.
It composes a prompt, calls `codex queue` directly, and responds when the host
delivers that prompt as user input in the same conversation. Each next prompt is
chosen from the context that develops.

The prompt may be a question, instruction, definition, or other useful context.
The assistant chooses both its wording and form to suit the task.

## How it works

```mermaid
flowchart TD
    A[Establish goal and prompt allowance] --> B[Assistant composes a prompt]
    B --> C[Call codex queue for the current thread]
    C --> D[End turn and wait for delivery]
    D --> E[Address prompt using accumulated context]
    E --> F{More inquiry useful and allowed?}
    F -->|Yes| B
    F -->|No| G[Deliver the result and return to the task]
    classDef default fill:#e0f2fe,stroke:#0284c7,color:#0c4a6e;
```

Codex supplies model execution and message delivery. The skill guides the
assistant in choosing prompts and using their responses to clarify the task,
reconsider assumptions, or bring a wandering conversation back to its goal.

## Use

The assistant may initiate an inquiry when it would help the current task. You
can also ask explicitly:

> Use $meta-prompting to develop an approach to [your problem]. Allow up to four
> prompts and stop when there is a concrete next action.

The request has four parts:

| Part | Example | Purpose |
| --- | --- | --- |
| Skill instruction | `Use $meta-prompting` | Selects the skill |
| Problem statement | `develop an approach to [your problem]` | Gives the inquiry a goal; replace the placeholder with your actual problem |
| Prompt allowance | `Allow up to four prompts` | Sets the maximum number of self-generated prompts, including the first |
| Stop condition | `stop when there is a concrete next action` | Describes when the inquiry has accomplished enough |

The allowance is a ceiling, not a target. The assistant can stop earlier when the
condition is met. You can omit the allowance and stop condition and let the
assistant choose them from the task. Each queued prompt starts a subsequent
conversation turn; the allowance counts those prompts, not tool calls.


## Requirements

The host must provide a shell tool, `codex queue`, and `CODEX_THREAD_ID` for the
active conversation. The skill uses the existing Codex session; it needs no
separate model or API key.

## Prompt delivery

The assistant operates the queue; you do not need to paste its prompts. For example,
it can submit a prompt using Bash:

```bash
codex queue --thread "${CODEX_THREAD_ID:?Current thread is unavailable}" \
  --message 'What observation would let the user recognize success?'
```

The assistant then ends its turn. A successful command establishes acceptance;
actual arrival must be observed in the conversation before continuing.

## How Codex finds the skill

Codex discovers project skills in `.agents/skills/` and personal skills in
`~/.agents/skills/`. Install the `meta-prompting` folder in one of those locations
as described below.

`agents/openai.yaml` enables automatic selection. This lets the assistant choose
meta-prompting when it would help your task. You can still request it explicitly
with `$meta-prompting`. If it is missing from the skill selector, restart Codex,
or ask the assistant to read [SKILL.md](SKILL.md) and follow it directly.

## Install in another project

Install the package by copying one folder:

1. Download or clone [adamjpatterson/meta-prompting](https://github.com/adamjpatterson/meta-prompting).
2. In the project where you want to use the skill, create `.agents/skills/` if it
   does not already exist.
3. Copy `skills/meta-prompting/` from this repository into that directory. Copy
   the whole folder, including `LICENSE`; the result should look like this:

   ```text
   your-project/
     .agents/
       skills/
         meta-prompting/
           SKILL.md
           README.md
           LICENSE
           agents/
           references/
   ```

4. Open Codex in the destination project and look for `meta-prompting` in
   `/skills`, or type `$meta-prompting`. Restart Codex if it does not appear.

Copy the source folder in step 3, not this repository's symbolic link. If the
destination already contains `meta-prompting`, review that installation before
replacing it.

To make the skill available across your projects instead, copy the same folder
to `~/.agents/skills/meta-prompting/`. Choose either a project installation or a
personal installation to avoid duplicate entries.

### Try it

Ask the assistant to check that `codex queue --help` works in its execution
environment, then try:

> Use $meta-prompting for a one-prompt delivery check. Queue the question
> “What is the purpose of this skill?” to this conversation, respond when it
> arrives, then stop without queueing another prompt.

You should see the question arrive as a new user-input message in the same
conversation, followed by the assistant's response. The inquiry should end there.

## Compatibility and validation

Development checks used Codex CLI **0.169.0** on Linux with Bash and an active
Codex conversation. See the [validation notes](references/validation.md) for the
checks performed and their results.

### If queueing is blocked

`codex queue` needs to communicate with the local Codex app server. A restricted
execution environment may block access to the server's local files or connection.
In our development environment, the command worked when run with broader host
permissions. An explicit error such as `failed to start embedded app server:
Read-only file system` occurs during startup, before message submission. The
skill instructs the assistant to use the host's normal permission process to
resolve that failure and retry the same prompt once. The error need not identify
the exact write path to distinguish this startup failure from an uncertain send.

A timeout or lost connection can leave submission uncertain. In that case, the
assistant should inspect delivery evidence rather than automatically resending.
If the host cannot grant the needed access, the assistant reports the blocker.

## State and stopping

The skill instructs the assistant to keep the goal, prompt allowance, and pending
prompt in the conversation. It calls for one pending prompt at a time and stopping
when the goal is met or the allowance is exhausted. The assistant tracks the
allowance itself.

If sending fails with an uncertain outcome, the skill instructs the assistant
to stop instead of retrying automatically: the prompt may already have been
accepted. It also instructs the assistant to respect new user directions,
including a request to stop while a prompt is pending.

## Package

| File | Purpose |
| --- | --- |
| [SKILL.md](SKILL.md) | Assistant-facing workflow and stopping instructions |
| [agents/openai.yaml](agents/openai.yaml) | Display metadata and automatic selection |
| [references/example.md](references/example.md) | Illustrative inquiry |
| [references/validation.md](references/validation.md) | Validation scope and results |
| [LICENSE](LICENSE) | MIT license for the standalone package |

The skill folder can be copied without depending on other files in this repository.
