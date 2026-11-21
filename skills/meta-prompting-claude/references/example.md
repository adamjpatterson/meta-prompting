# An illustrative inquiry

This Claude Code example shows how each response can suggest the next prompt.

The user wants to improve a document but has not defined what “better” means. The assistant establishes a two-prompt allowance and a stopping condition: one specific revision and an observable way to judge it.

It checks the local time and schedules a one-shot prompt for the next minute:

> What must the reader be able to do after reading this document?

After ending its turn and receiving the prompt, the assistant identifies a useful criterion: the reader should be able to start the workflow without confusing an example command with a complete execution. That finding suggests the next prompt:

> Which part of the document leaves that action ambiguous?

The assistant schedules that prompt and ends the turn again. On arrival, it identifies a passage to revise and proposes a check: ask a reader to locate and explain the first action using only the revised document.

Both prompts have been used. The assistant delivers the recommendation and check, then returns to the original task. The count and outcome remain in the conversation.

This illustrates intended multi-prompt behavior; the recorded Claude Code live check covered one prompt.
