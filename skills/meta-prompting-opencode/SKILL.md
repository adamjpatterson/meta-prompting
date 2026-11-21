---
name: meta-prompting-opencode
license: MIT
description: In OpenCode, proactively reframe work when attempts repeat without progress, an untested premise drives the approach, complexity grows without clarifying the goal, or the conversation drifts. Construct useful context through self-generated prompts in the active conversation. Also use for explicit meta-prompting or recursive self-prompting requests.
compatibility: Requires OpenCode, shell access, opencode run, and a verified active session ID.
metadata:
  version: "0.2.0-dev"
---

# Meta-prompting for OpenCode

## Setup

Compose whatever context would help the current task; choose each subsequent prompt from the developing conversation.

Briefly state what the inquiry should clarify and choose a finite prompt allowance unless the user supplied one. Track the goal, pending prompt, and count in the conversation; the first prompt counts toward the allowance.

## Delivery

Requires a shell tool and an active OpenCode session ID. If either is unavailable, explain what is missing; do not substitute another session.

Identify the session with `opencode session list`, corroborated by `opencode export <session-id>` matching this conversation.

Replace the placeholders with the verified session ID and your prompt, and run in Bash:

```bash
opencode run -s '<active-session-id>' 'Your prompt text'
```

Pass the exact prompt as one safely quoted argument; avoid shell expansion.

`opencode run` is synchronous: it blocks until the response, so the prompt arrives and the reply is produced inside the same call. Count each call as one prompt before invoking it. If it hangs or times out, do not resubmit — the input may already be admitted; stop and report uncertainty.

The calling agent owns continuation. In every submitted prompt, instruct the receiving execution to answer without submitting another prompt. After the call returns, use its response to decide whether to continue; do not answer that prompt a second time. Compose any next prompt from the returned response and call `opencode run -s` again on the same session, keeping one prompt pending at a time and counting each call once.

Respect newer user directions; a delayed prompt must not restart a stopped inquiry. If the pending prompt, goal, or count is unclear, stop and explain the uncertainty. If submission is uncertain, inspect delivery evidence rather than automatically resending.

## Completion

Stop when the inquiry has clarified enough, a user-specified stopping condition is met, further prompts would not help, or the allowance is exhausted. Missing information is a reason to stop when further inquiry cannot help without it. After the final allowed prompt has been answered, return to the user's task and apply or report the useful result. Do not automatically reset the allowance.

See [the example](references/example.md) for an illustrative inquiry.
