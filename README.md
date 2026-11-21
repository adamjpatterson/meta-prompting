# Meta-prompting

Skills for constructing useful context through self-generated prompts in the active conversation.

## Introduction

This is an experimental skill aimed at exploring how instruction hierarchy influences context. Each message in the conversation is associated with a particular role (system, developer, user, assistant, tool, and so on). These roles carry different levels of authority within the model's instruction hierarchy.

This skill lets the agent submit its self-generated prompts under the user role rather than the assistant role. This gives those prompts greater instructional priority than they would have as ordinary assistant messages.[^1]

The hypothesis is that meta-prompting may allow the agent to correct unproductive trajectories. However, self-generated prompts may also reinforce a mistaken framing.

[^1]: The presumed increase in instructional priority has not been independently verified and depends on how the host presents queued messages to the model.

## Implementations

Choose the package for your client:

- [Codex installation and usage](docs/codex.md)
- [OpenCode installation and usage](docs/opencode.md)

### Agent Skills specification

Both skills follow the [Agent Skills specification](https://agentskills.io/specification), with client-specific instructions for Codex and OpenCode.

## License

[MIT](LICENSE).
