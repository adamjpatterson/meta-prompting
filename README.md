# Meta-prompting

Skills for constructing useful context through self-generated prompts in the active conversation.

## Introduction

This is an experimental skill aimed at exploring how instruction hierarchy influences context. Each message in the conversation is associated with a particular role (system, developer, user, assistant, tool, and so on). These roles carry different levels of authority within the model's instruction hierarchy.

This skill lets the agent submit its self-generated prompts under the user role rather than the assistant role. This gives those prompts greater instructional priority than they would have as ordinary assistant messages.[^1]

The hypothesis is that meta-prompting may allow the agent to correct unproductive trajectories. However, self-generated prompts may also reinforce a mistaken framing.

[^1]: The presumed increase in instructional priority has not been independently verified and depends on how the host presents queued messages to the model. Whether the host also displays the self-generated prompt to the user varies by client; each guide describes what you will see.

## Implementations

Choose the package for your client:

- [Codex installation and usage](docs/codex.md)
- [OpenCode installation and usage](docs/opencode.md)
- [Claude Code installation and usage](docs/claude.md)

### Agent Skills specification

All three skills follow the [Agent Skills specification](https://agentskills.io/specification), with client-specific instructions for Codex, OpenCode, and Claude Code.

## Credits

Developed by Adam Patterson in collaboration with OpenAI Codex. Codex contributed the original implementation, the Codex and OpenCode packages, documentation, validation, and release preparation. Claude Code (Fable 5.1) contributed the Claude Code package, with review from Codex. Mistral contributed to the OpenCode delivery research and testing.

## License

[MIT](LICENSE).
