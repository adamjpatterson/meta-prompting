# Schedule recovery

Distinguish a rejected call before scheduling from an accepted job that has not yet arrived.

If `CronCreate` is unavailable, denied by the permission settings, or disabled by the host (Claude Code honors `CLAUDE_CODE_DISABLE_CRON=1`), the prompt was never scheduled. Resolve the problem through the host's normal permission or configuration process, then retry the same prompt once. A confirmed rejection does not consume the prompt allowance. Do not resume the session from a shell, post to the session's messaging socket, or use another session to work around the rejection. If the retry fails, report the blocker.

If the call was accepted but the prompt has not arrived, it is pending, not lost. Scheduled prompts fire only while the session is idle, at one-minute granularity, and a job on a round minute may fire up to about ninety seconds early. End the turn and wait. Before any resend, call `CronList`: a listed job means the prompt is still pending, so do not schedule a duplicate. Delete a pending job with `CronDelete` only when the user has stopped the inquiry.

Scheduled jobs belong to the session. After a restart or resume, list jobs before assuming a pending prompt survived, and report what you find rather than resending automatically. Absence of a result message alone does not prove non-submission.
