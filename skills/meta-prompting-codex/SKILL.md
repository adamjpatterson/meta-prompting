---
name: meta-prompting-codex
license: MIT
description: In Codex, proactively reframe work when attempts repeat without progress, an untested premise drives the approach, complexity grows without clarifying the goal, or the conversation drifts. Construct useful context through self-generated prompts in the active conversation. Also use for explicit meta-prompting or recursive self-prompting requests.
compatibility: Requires Codex, a shell tool, codex queue, and CODEX_THREAD_ID for the active conversation.
metadata:
  version: "0.2.0-dev"
---

# Meta-prompting for Codex

## Setup

Compose whatever context would help the current task; choose each subsequent prompt from the developing conversation.

Briefly state what the inquiry should clarify and choose a finite prompt allowance unless the user supplied one. Track the goal, pending prompt, and count in the conversation; the first prompt counts toward the allowance.

## Delivery

Requires a shell tool, `codex queue`, and the host-provided `CODEX_THREAD_ID`. If any is unavailable, explain what is missing; do not substitute another thread.

Replace the placeholder with your prompt and run in Bash:

```bash
codex queue --thread "${CODEX_THREAD_ID:?Current thread is unavailable}" \
  --message 'Your prompt text'
```

Avoid shell expansion of prompt text and do not use `--resume`. After queue acceptance, update the count and end the turn with a brief handoff. Wait for arrival as user input before responding; acceptance is not arrival. Respond as appropriate, then decide whether another prompt would help.

Keep one prompt pending at a time and count each submitted prompt once. Respect newer user directions; a delayed prompt must not restart a stopped inquiry. If the pending prompt, goal, or count is unclear, stop and explain the uncertainty. If submission is uncertain, inspect delivery evidence rather than automatically resending. For startup failures, see [queue recovery](references/queue-recovery.md).

## Completion

Stop when the inquiry has clarified enough, a user-specified stopping condition is met, further prompts would not help, or the allowance is exhausted. Missing information is a reason to stop when further inquiry cannot help without it. After the final allowed prompt has been answered, return to the user's task and apply or report the useful result. Do not automatically reset the allowance.

See [the example](references/example.md) for an illustrative inquiry.
