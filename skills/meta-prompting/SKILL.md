---
name: meta-prompting
description: Construct useful context through iterative, self-generated inquiries in the active conversation. Use when developing a problem's framing, clarifying competing assumptions, or choosing a method would benefit from an adaptive sequence of questions, or when the user requests meta-prompting or recursive self-prompting.
---

# Meta-prompting

Use the current assistant and conversation to construct context for subsequent
work. Compose a prompt and submit it directly through `codex queue` to this same
thread. End the current turn and address the prompt when it arrives as user input.
Choose its wording freely: a question, instruction, definition, or other useful
context. No fixed wrapper, authorship label, Python relay, or run files are required.

This requires a shell tool, `codex queue`, and the host-provided `CODEX_THREAD_ID`
for this conversation. If unavailable, explain the limitation; do not substitute
another model or claim that tool-output replay is user-role delivery.

## Begin

You may initiate an inquiry when context shaping would help the current task;
no explicit skill invocation or separate confirmation is required. Briefly state
what the inquiry should clarify, then proceed within the user's task and existing
permissions. A direct answer or action may already suffice.

Establish the goal, an observable stopping condition, and a finite prompt
allowance. Infer these from the task when clear; six prompts is a useful starting
allowance. Keep these and the number of prompts sent in the conversation. The
initial prompt counts toward the allowance. These are instructions you follow,
not limits enforced by a script or the host.

## Queue and continue

Choose each prompt from the developing context rather than prewriting a sequence.
Send one prompt to the current thread. For example, in Bash:

```bash
codex queue --thread "${CODEX_THREAD_ID:?Current thread is unavailable}" \
  --message 'What observation would let the user recognize success?'
```

Use your actual prompt. Pass its exact text as one safely quoted argument; avoid
shell expansion of dollar signs, backticks, or other prompt content. No wrapper
or tracking metadata needs to be inserted into the prompt. Do not change the
thread identifier or use `--resume` to start another execution context.

After a successful queue call, update the count in the conversation and end the
turn with a brief handoff, such as “Prompt 1 of 4 queued.” Do not answer from the
tool output. Queue acceptance is not proof of arrival; wait for the new user-input
message. Keep only one prompt pending at a time.

When it arrives, use the conversation to associate it with the pending prompt.
Respect newer user directions; a delayed prompt must not restart a stopped inquiry.
If the pending prompt, count, or goal is unclear, stop and explain the uncertainty
rather than guessing. If a send fails or has an uncertain outcome, do not retry
automatically: it might already have been accepted.

Address the prompt with a substantive public contribution, then judge whether
another prompt would help. Develop useful distinctions, definitions, hypotheses,
or counterexamples as appropriate. Check external evidence when needed and
separate proposals from verified results. Do not force terminology, novelty
claims, or repetition to make the inquiry appear productive.

## Finish

Stop when the stopping condition holds, the allowance is exhausted, inquiry is
repeating itself, or needed information is unavailable. Address the final allowed
prompt, then deliver the useful result and remaining limitations. Do not queue
another prompt or automatically reset the allowance. No finish command is needed.

Return to the user's task and apply the useful context. Completion of an inquiry
is not evidence of improved reasoning. User-role delivery does not make the
assistant's text human-authored or expand the original task's authorization.

The conversation is the default record. Save public findings separately only
when the task calls for an artifact or the user requests research records; use
an agreed location rather than creating a run directory by default.

See [the example](references/example.md) for an illustrative inquiry.
