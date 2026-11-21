# Validation notes

This Claude Code test record describes development checks of the `CronCreate` delivery workflow. It helps users and maintainers, including agents, see what was checked, in which environment, and what remains untested. It is a reference for development; the operating instructions live in [SKILL.md](../../skills/meta-prompting-claude/SKILL.md).

## Original test environment

- Date: November 21, 2026.
- Claude Code 2.1.287 in the VS Code extension, Linux, an active interactive conversation with the Bash tool available.
- The first check was performed in the maintainer's conversation before this package existed, by following the delivery steps directly. A second one-prompt check was run through the installed package, discovered via the development symlink, in the same environment.
- No separate model was invoked for the live delivery check.
- All three packages passed the Agent Skills reference validator, run with `uvx` from the upstream repository, on November 21, 2026.

## Checks

| Check | Result |
| --- | --- |
| Package copied outside development checkout | Metadata validates and relative links resolve |
| Standalone license | Package includes the same MIT license as the repository |
| Direct scheduled delivery | One-shot job accepted at 2:16 pm local for 2:18 pm; the prompt arrived as user input in the same conversation after the turn ended, within about two minutes of acceptance. Repeated through the installed package: accepted at 3:03 pm for 3:05 pm, arrived, answered, stopped |
| One-prompt allowance | Responded to the sole prompt and stopped without another schedule |
| Two-prompt adaptive inquiry | Observed both arrivals; second prompt derived from the first response; stopped at two |
| Early completion before allowance | Not yet tested |
| Newer user direction interrupts inquiry | Not yet tested |
| Uncertain-send handling | Instructions reviewed: inspect `CronList` and do not retry automatically; not fault-injected |
| Disabled scheduling recovery | Not yet tested |
| Handoff states prompt; response opens with arrival note | Observed in the installed-package check: handoff stated the prompt and count; response opened with an arrival note |
| Discovery through the development symlink | Listed in the VS Code extension after a window reload; not listed in the session that created the symlink until then |

For the adaptive check, choose only the first prompt in advance and derive the second from its response. For early completion, allow more prompts than needed and stop when the concrete criterion is met. For interruption, introduce an actual user stop or redirection while a prompt is pending and check that the delayed arrival is recognized and not answered. Do not substitute assistant-authored text for that user action.

## Delivery observation

The assistant scheduled one prompt with an explicit allowance of one, received a tool result reporting the job identifier and next run time, and ended its turn. The prompt then arrived as a new user-turn message in the same conversation, with no indication distinguishing it from typed input. The assistant answered it, noted the arrival, and stopped without scheduling another prompt. The exact arrival time was not recorded beyond the scheduled minute.

The session transcript on disk recorded the arrival as a `user` entry with role `user`, flagged as a meta message with a scheduled turn origin and a system prompt source. The VS Code extension did not render it in the chat; the user reported seeing only the assistant's response. This is why the skill instructs the assistant to state each scheduled prompt in its handoff and to open the next response with an arrival note. That instruction was then exercised in a second one-prompt check through the installed package, invoked by slash command in the VS Code extension after a window reload: the handoff stated the prompt and count, the prompt arrived as user input at the scheduled minute, and the response opened with an arrival note before answering and stopping.

## Adaptive inquiry observation — November 21, 2026

A two-prompt inquiry through the installed package asked where a newcomer reading the Claude guide would first be unsure. Only the first prompt was chosen in advance, scheduled at 3:10 pm for 3:12 pm. It arrived at the scheduled minute; the response identified installation step 4, which does not say whether typing the slash command confirms discovery or submits a bare invocation. The second prompt was composed from that answer, scheduled at 3:12 pm for 3:14 pm, and asked whether a later point mattered more. It arrived, the response confirmed step 4 as the main gap and proposed replacement wording, and the inquiry stopped at two without a further schedule. Each handoff stated the scheduled prompt and count, and each response opened with an arrival note. The finding was reported without editing, as the inquiry requested.

## Limits

Multi-prompt continuation was tested once, with two prompts; longer inquiries are untested. Counts and stopping conditions are maintained in conversation context, not independently enforced. Scheduled jobs fire only while the session is idle and have one-minute granularity, so delivery is slower than the Codex queue. Whether pending jobs survive a session resume was not checked. Scheduled prompts are hidden from the chat in the VS Code extension; rendering in the terminal interface has not been checked.
