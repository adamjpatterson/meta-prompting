# Queue recovery

Distinguish a confirmed failure before submission from uncertain acceptance.

If `codex queue` explicitly fails to start its app server, such as with a read-only-filesystem error, resolve the startup problem through the host's normal permission or approval process, then retry the same prompt once. A confirmed pre-submission failure does not consume the prompt allowance. Do not invent the blocked path or change the thread or Codex home to work around the error. If the retry fails, report the blocker.

If the command times out, loses its connection after submission may have begun, or otherwise leaves acceptance uncertain, do not resend automatically: the prompt might already be queued. Inspect available delivery evidence before deciding how to recover. Absence of a success message alone does not prove non-submission.
