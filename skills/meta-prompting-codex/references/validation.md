# Validation notes

This Codex test record describes development checks of the direct `codex queue` workflow. It helps users and maintainers, including agents, see what was checked, in which environment, and what remains untested. It is a reference for development; the operating instructions live in [SKILL.md](../SKILL.md).

## Original test environment

- Date: November 18, 2026.
- Codex CLI: 0.169.0, Linux, Bash, an existing active Codex conversation.
- Queue delivery succeeded after allowing the command broader host access to communicate with the local Codex app server.
- No separate model was invoked for the live delivery checks.

## Checks

| Check | Result |
| --- | --- |
| Package copied outside development checkout | Metadata validates and relative links resolve |
| Standalone license | Package includes the same MIT license as the repository |
| Startup permission recovery | Observed delivery after an app-server read-only-filesystem startup error was resolved using broader host permissions |
| Direct queue delivery | Observed user-role arrival and response in the same conversation |
| One-prompt allowance | Responded to the sole prompt and stopped without another send |
| Two-prompt adaptive inquiry | Observed both arrivals; second prompt derived from the first response; stopped at two |
| Early completion before allowance | Observed completion after one prompt with an allowance of three; no further prompt in that inquiry |
| Newer user direction interrupts inquiry | User stop received after queue acceptance; delayed prompt arrived and was disregarded without continuing the inquiry |
| Uncertain-send handling | Instructions reviewed: stop and do not retry automatically; not fault-injected |

For the adaptive check, choose only the first prompt in advance and derive the second from its response. For early completion, allow more prompts than needed and stop when the concrete criterion is met. For interruption, introduce an actual user stop or redirection while the inquiry is active and check that it is respected. Do not substitute assistant-authored text for that user action.

## Adaptive inquiry observation

A two-prompt inquiry examined installation guidance. The first response separated skill discovery from delivery to the active conversation. The second prompt was then composed to ask for a minimal check of delivery, response, and stopping. Both arrived as user input in the same conversation. The inquiry ended at its two-prompt allowance and produced the one-prompt first-use check now documented in the Codex guide.

## Early-completion observation

A separate inquiry allowed up to three prompts and asked for an observable first-use success criterion and a limitation. Both were supplied after the first queued prompt arrived. The inquiry concluded without using its remaining two prompts. The prompt explicitly invited early completion, so this is a check of following the stopping instruction, not a test of unprompted stopping judgment.

## Interruption observation

One inquiry prompt was queued, then the user sent an actual stop instruction before its arrival. The assistant canceled the inquiry and resumed release preparation. The queued prompt subsequently arrived as user input and was recognized as belonging to the canceled inquiry. It was not answered and no further inquiry prompt was sent. This checks one observed ordering of stop and delivery, not every possible timing or context-loss scenario.

## Shortened instructions — November 21, 2026

- An independent subagent read the shortened skill and described a one-prompt workflow, including waiting for arrival and responding before stopping. This was a dry run; it sent no messages and found no missing mechanics.
- A live one-prompt check in the active Codex conversation observed queue acceptance, arrival as user input, a response, and stopping without another send. This exercised the shortened instructions, not just their interpretation.
- Separate subagents compared the current description with a capability-first alternative. Both selected the skill in an initial reframing scenario. In a second simulation with six scenarios and five available skills, both selected meta-prompting for stalled diagnosis and unfocused onboarding, and not for the four other tasks. These were stated choices, not observed activation during task execution; the tests explicitly asked about selection and used a small catalog. They showed no advantage for the alternative description.

## GitHub installation and first use — November 21, 2026

A separate Codex session in `meta-prompting-codex-test` followed the project-local installation and first-use instructions in the client guide. Evidence was supplied by the user as a GUI screenshot and the resulting conversation transcript.

- The installer reported downloading `skills/meta-prompting-codex` from `adamjpatterson/meta-prompting` at `main` into the project's `.agents/skills/meta-prompting-codex` directory.
- The GUI skill selector displayed “Meta-prompting for Codex” with Project scope.
- `codex queue --help` exited successfully, with a nonfatal read-only-filesystem warning about PATH aliases.
- The first delivery attempt failed during embedded app-server startup. The agent followed the recovery instructions and retried through the normal permission process without consuming the prompt allowance for the failed attempt.
- The queued question appeared as a new user-input message in that conversation. The agent answered it and stopped at one prompt without another submission.

This checks the documented GitHub installation, discovery, startup recovery, and one-prompt workflow for the renamed package. The installed CLI version was not separately recorded for this check.

## Limits

Counts and stopping conditions are maintained in conversation context, not independently enforced. Context loss, ambiguous delivery, or different host behavior can prevent continuation. Cross-host compatibility, long inquiries, and automatic-selection reliability have not been systematically evaluated.

These checks exercise delivery and inquiry control. They do not compare reasoning outcomes or isolate the effect of user-role delivery from deliberate reframing or an additional conversation turn.
