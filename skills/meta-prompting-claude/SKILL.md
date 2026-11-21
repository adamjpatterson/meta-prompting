---
name: meta-prompting-claude
license: MIT
description: In Claude Code, proactively reframe work when attempts repeat without progress, an untested premise drives the approach, complexity grows without clarifying the goal, or the conversation drifts. Construct useful context through self-generated prompts in the active conversation. Also use for explicit meta-prompting or recursive self-prompting requests.
compatibility: Requires Claude Code in an interactive session with the CronCreate scheduling tool available.
metadata:
  version: "0.3.0"
---

# Meta-prompting for Claude Code

## Setup

Compose whatever context would help the current task; choose each subsequent prompt from the developing conversation.

Briefly state what the inquiry should clarify and choose a finite prompt allowance unless the user supplied one. Track the goal, pending prompt, and count in the conversation; the first prompt counts toward the allowance.

## Delivery

Requires the `CronCreate` tool in an interactive Claude Code session. If the tool is unavailable or scheduling is disabled, explain what is missing; do not substitute another session, resume this session from a shell, or post to the session's messaging socket.

Check the current local time, then call `CronCreate` with your prompt, `recurring` set to `false`, and a cron expression pinned to the next whole minute: minute, hour, day-of-month, and month set to that time, day-of-week `*`. For example, at 2:16 pm on November 21:

```text
CronCreate
  prompt: "Your prompt text"
  cron: "18 14 21 11 *"
  recurring: false
```

The tool result confirms acceptance and reports the next run time. Scheduled prompts fire only while the session is idle, at one-minute granularity. After acceptance, update the count and end the turn with a brief handoff that states the scheduled prompt verbatim and the count, because Claude Code does not display scheduled prompts in the conversation. Wait for arrival as user input before responding; acceptance is not arrival. Open the response with a one-line arrival note naming the prompt number, answer as appropriate, then decide whether another prompt would help.

Keep one prompt pending at a time and count each accepted prompt once. Respect newer user directions; a delayed prompt must not restart a stopped inquiry. If the pending prompt, goal, or count is unclear, stop and explain the uncertainty. If submission is uncertain, inspect `CronList` rather than automatically resending. For rejected calls and missing arrivals, see [schedule recovery](references/schedule-recovery.md).

## Completion

Stop when the inquiry has clarified enough, a user-specified stopping condition is met, further prompts would not help, or the allowance is exhausted. Missing information is a reason to stop when further inquiry cannot help without it. After the final allowed prompt has been answered, return to the user's task and apply or report the useful result. Do not automatically reset the allowance.

See [the example](references/example.md) for an illustrative inquiry.
