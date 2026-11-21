# Validation notes

This Claude Code test record describes development checks of the `CronCreate` delivery workflow. It helps users and maintainers, including agents, see what was checked, in which environment, and what remains untested. It is a reference for development; the operating instructions live in [SKILL.md](../SKILL.md).

## Original test environment

- Date: November 21, 2026.
- Claude Code 2.1.287 in the VS Code extension, Linux, an active interactive conversation with the Bash tool available.
- The check was performed in the maintainer's conversation before this package existed, by following the delivery steps directly. The installed package had not yet been loaded by the assistant.
- No separate model was invoked for the live delivery check.
- All three packages passed the Agent Skills reference validator, run with `uvx` from the upstream repository, on November 21, 2026.

## Checks

| Check | Result |
| --- | --- |
| Package copied outside development checkout | Metadata validates and relative links resolve |
| Standalone license | Package includes the same MIT license as the repository |
| Direct scheduled delivery | One-shot job accepted at 2:16 pm local for 2:18 pm; the prompt arrived as user input in the same conversation after the turn ended, within about two minutes of acceptance |
| One-prompt allowance | Responded to the sole prompt and stopped without another schedule |
| Two-prompt adaptive inquiry | Not yet tested |
| Early completion before allowance | Not yet tested |
| Newer user direction interrupts inquiry | Not yet tested |
| Uncertain-send handling | Instructions reviewed: inspect `CronList` and do not retry automatically; not fault-injected |
| Disabled scheduling recovery | Not yet tested |
| Discovery through the development symlink | Not yet checked; a subagent started in the same session did not list the skill, but subagents inherit the session's startup skill snapshot, so this is not evidence either way |

For the adaptive check, choose only the first prompt in advance and derive the second from its response. For early completion, allow more prompts than needed and stop when the concrete criterion is met. For interruption, introduce an actual user stop or redirection while a prompt is pending and check that the delayed arrival is recognized and not answered. Do not substitute assistant-authored text for that user action.

## Delivery observation

The assistant scheduled one prompt with an explicit allowance of one, received a tool result reporting the job identifier and next run time, and ended its turn. The prompt then arrived as a new user-turn message in the same conversation, with no indication distinguishing it from typed input. The assistant answered it, noted the arrival, and stopped without scheduling another prompt. The exact arrival time was not recorded beyond the scheduled minute.

## Limits

Multi-prompt continuation has not been tested on Claude Code; the package assumes that a second scheduled prompt behaves like the first. Counts and stopping conditions are maintained in conversation context, not independently enforced. Scheduled jobs fire only while the session is idle and have one-minute granularity, so delivery is slower than the Codex queue. Whether pending jobs survive a session resume was not checked. Role presentation was observed in the conversation, not confirmed in the stored transcript.

These checks exercise delivery and stopping. They do not compare reasoning outcomes or isolate the effect of user-role delivery from deliberate reframing or an additional conversation turn.
