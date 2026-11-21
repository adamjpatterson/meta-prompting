---
name: meta-prompting
license: MIT
description: Proactively reframe work when attempts repeat without progress, an untested premise drives the approach, complexity grows without clarifying the goal, or the conversation drifts. Construct useful context through self-generated prompts in the active conversation. Also use for explicit meta-prompting or recursive self-prompting requests.
compatibility: Requires Codex with a shell tool, codex queue, and CODEX_THREAD_ID for the active conversation.
metadata:
  version: "0.1.1"
---

# Meta-prompting

## Setup

Compose whatever context would help the current task; choose each subsequent
prompt from the developing conversation.

Briefly state what the inquiry should clarify and choose a finite prompt allowance
unless the user supplied one. Track the goal, pending prompt, and count in the
conversation; the first prompt counts toward the allowance.

## Delivery

Replace the placeholder with your prompt and send it to the current thread using Bash:

```bash
codex queue --thread "${CODEX_THREAD_ID:?Current thread is unavailable}" \
  --message 'Your prompt text'
```

Pass the exact prompt as one safely quoted argument, avoiding shell expansion.
Use the current thread without `--resume`. If the required tools or thread
identifier are unavailable, explain what is missing.

Keep one prompt pending at a time. After queue acceptance, update the count and
end the turn with a brief handoff. Wait for the prompt to arrive as user input;
the command result confirms acceptance, not arrival. On arrival, respond as
appropriate, then decide whether another prompt would help.

Respect newer user directions; a delayed prompt must not restart a stopped inquiry.
If the pending prompt, goal, or count is unclear, stop and explain the uncertainty.
If submission is uncertain, inspect delivery evidence rather than automatically
resending. For command failures, consult [queue recovery](references/queue-recovery.md)
before retrying.

## Completion

Stop when the inquiry has clarified enough, a user-specified stopping condition
is met, further prompts would not help, or the allowance is exhausted. Missing
information is a reason to stop when further inquiry cannot help without it.
Respond to the final allowed prompt, then return to the user's task and apply or
report the useful result. Do not automatically reset the allowance.

See [the example](references/example.md) for an illustrative inquiry.
