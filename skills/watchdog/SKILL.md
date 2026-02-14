# Watchdog Skill

A keep-alive monitor for long-running tasks. Use this to reassure the user that a heavy operation is still in progress.

## Usage

1. Call `watchdog_start` BEFORE a heavy tool call (like `sessions_spawn` or a long `exec`).
2. Call `watchdog_stop` IMMEDIATELY AFTER the heavy tool call returns.

## Tools

### `watchdog_start`
Start sending periodic "Still working" messages to the user.
- `interval_minutes`: How often to message (default: 5).
- `message`: Custom status text (default: "🐺 Still working on your task...").
- `target`: The channel/user to notify (defaults to current context if possible, otherwise provide explicit `telegram:<id>`).

```javascript
// Implementation note: This creates an OpenClaw Cron Job targeting an isolated session.
// The isolated agent simply sends the message and exits.
const jobId = "watchdog-monitor";
const everyMs = interval_minutes * 60 * 1000;

await tools.cron.add({
  jobId: jobId,
  schedule: { kind: "every", everyMs: everyMs },
  sessionTarget: "isolated",
  payload: {
    kind: "agentTurn",
    message: `Use the message tool to send this exact text to the user: "${message}". Do not add anything else.`,
    model: "google/gemini-3-pro-preview" // fast/cheap model
  },
  delivery: {
    mode: "auto" // Should route to the last active channel
  }
});
return "Watchdog started.";
```

### `watchdog_stop`
Stop the keep-alive monitor.

```javascript
const jobId = "watchdog-monitor";
try {
  await tools.cron.remove({ jobId: jobId });
  return "Watchdog stopped.";
} catch (e) {
  return "Watchdog already stopped or not found.";
}
```
