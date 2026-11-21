# An illustrative inquiry

The user wants a clearer onboarding document. The calling agent chooses a two-prompt allowance and submits a prompt asking what the reader needs to accomplish, instructing the receiving execution to answer without sending another prompt.

The synchronous call returns a response. The caller uses it to compose a second prompt asking which passage prevents that outcome, again requesting an answer without further submissions. When that response returns, the caller reports the proposed revision and stops at two prompts. It does not answer either prompt again.

This illustrates intended multi-prompt behavior; the recorded OpenCode live check covered one prompt.
