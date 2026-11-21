# OpenCode validation notes

The following observations predate the package split. The separate package and explicit caller-owned continuation have not yet been live-tested.

## Experimental OpenCode reference

The earlier combined-package experiment added client routing and an OpenCode reference based on a user-supplied report from OpenCode 1.19.6. That report describes synchronous `opencode run -s` execution and exported user/assistant messages in the same session. These observations were not independently reproduced here. The earlier OpenCode-to-Codex injection does not count as OpenCode self-delivery.

The revised reference was subsequently exercised by the one-prompt check below. Exact scheduler ordering and multi-prompt continuation remain unverified.

## Live OpenCode one-prompt check — November 21, 2026

- Environment: OpenCode 1.19.6, Linux, Bash, active session `ses_e0112ce32ffexoNnXd3MHjUoDF` ("Testing meta-prompting skill"), identified via `opencode session list` and corroborated by `opencode export` matching this conversation.
- One prompt with an explicit allowance of 1 was submitted with `opencode run -s`. The call blocked and returned the response synchronously.
- `opencode export` afterward showed the prompt exactly once as `user` and the reply exactly once as `assistant` in the same session. No further prompt was sent.
- This confirms the synchronous one-prompt flow: session identification, submission, arrival, response, exactly-once export check, and stopping at the allowance. Multi-prompt continuation on OpenCode remains unverified.

## Limits

The one-prompt result was reported from OpenCode; it was not independently reproduced by the Codex maintainer. Multi-prompt continuation and exact scheduler ordering remain unverified.
