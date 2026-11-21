---
name: meta-prompting
description: Proactively reframe work when attempts repeat without progress, an untested premise drives the approach, complexity grows without clarifying the goal, or the conversation drifts. Construct useful context through self-generated prompts in the active conversation. Also use for explicit meta-prompting or recursive self-prompting requests.
compatibility: Requires Codex with a shell tool, codex queue, and CODEX_THREAD_ID for the active conversation.
---

# Meta-prompting

Use the current assistant and conversation to construct context for subsequent
work. Compose a prompt and submit it directly through `codex queue` to this same
thread. End the current turn and address the prompt when it arrives as user input.
Choose its wording freely: a question, instruction, definition, or other useful
context. Adapt the form and framing to whatever would help the task.

This requires a shell tool, `codex queue`, and the host-provided `CODEX_THREAD_ID`
for this conversation. If unavailable, explain what is missing.

## Take initiative

Take responsibility for the direction of the inquiry as well as the immediate
response. When you notice repeated attempts without progress, an untested premise
driving the approach, growing complexity without a clearer goal, or drift from
the user's objective, initiate a brief meta-prompting inquiry before continuing
along the same trajectory. Do not wait for the user to name the skill or point
out the framing problem. A routine task with a clear approach can proceed directly.

Preserve the user's objective while reconsidering the framing, including
assumptions you introduced. Treat a proposed solution as a candidate rather than
silently turning it into a requirement. Distinguish that from an explicit user
constraint. Briefly state what the inquiry should clarify, then proceed without
separate confirmation.

Compose the context that would make the next step more productive. You may define
a distinction, name or map concepts, explore an alternative, challenge a premise,
or formulate a better question. These are possibilities, not a required sequence.
Revisit or discard your own constructions when they stop helping; consistency
with an earlier response is not a reason to preserve a mistaken assumption.

## Begin

Establish the goal, an observable stopping condition, and a finite prompt
allowance. Infer these from the task when clear; six prompts is a useful starting
allowance. Keep these and the number of prompts sent in the conversation. The
initial prompt counts toward the allowance.

## Queue and continue

Choose each prompt from the developing context rather than prewriting a sequence.
Send one prompt to the current thread. For example, in Bash:

```bash
codex queue --thread "${CODEX_THREAD_ID:?Current thread is unavailable}" \
  --message 'What observation would let the user recognize success?'
```

Use your actual prompt. Pass its exact text as one safely quoted argument; avoid
shell expansion of dollar signs, backticks, or other prompt content. Send to the
current thread without `--resume`.

After a successful queue call, update the count in the conversation and end the
turn with a brief handoff, such as “Prompt 1 of 4 queued.” Wait until the queued
prompt arrives as a new user-input message before answering it. The command
result only confirms queue acceptance. Keep only one prompt pending at a time.

When it arrives, use the conversation to associate it with the pending prompt.
Respect newer user directions; a delayed prompt must not restart a stopped inquiry.
If the pending prompt, count, or goal is unclear, stop and explain the uncertainty
rather than guessing.

Distinguish failure before submission from an uncertain delivery outcome. If
`codex queue` explicitly fails to start its app server (for example, with a
read-only-filesystem error), resolve the startup problem through the host's normal
permission or approval process, then retry the same prompt once. A confirmed
pre-submission failure does not consume the prompt allowance. Do not invent the
blocked path or change the thread or Codex home to work around the error. If the
retry also fails, report the blocker rather than repeatedly retrying.

If the command times out, loses its connection after submission may have begun,
or otherwise leaves acceptance uncertain, do not resend automatically: the prompt
might already be queued. Inspect available delivery evidence before deciding how
to recover. Absence of a success message alone does not prove non-submission.

Address the prompt with a substantive public contribution, then judge whether
another prompt would help. Develop useful distinctions, definitions, hypotheses,
or counterexamples as appropriate. Explore alternative framings, revisit an
assumption, or reorient the conversation when useful. Check external evidence
when the task calls for it.

## Finish

Stop when the stopping condition holds, the allowance is exhausted, inquiry is
repeating itself, or needed information is unavailable. Address the final allowed
prompt, then deliver the useful result and remaining limitations. Do not queue
another prompt or automatically reset the allowance.

Return to the user's task and apply the useful context. Keep findings in the
conversation, or save them as an artifact when that helps the task.

See [the example](references/example.md) for an illustrative inquiry.
