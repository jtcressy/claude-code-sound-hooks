# Sound Hooks for Claude Code

A Claude Code plugin that plays audio notifications for all 26 hook events. Features a mix of novelty sound effects and voice lines — rubber ducky squeaks, Warcraft 3 peon voice lines ("Work work", "Something need doing?"), and other comedic audio cues. Each hook event has a distinct sound so you can tell what Claude is doing just by listening.

Sounds were generated with [ElevenLabs](https://elevenlabs.io/) TTS using the **Samara X** voice, mixed with various sound effects.

## Installation

Add the marketplace to your `~/.claude/settings.json`:

```json
{
  "extraKnownMarketplaces": {
    "jtcressy-claude-plugins": {
      "source": {
        "source": "github",
        "repo": "jtcressy/claude-code-sound-hooks"
      },
      "autoUpdate": true
    }
  },
  "enabledPlugins": {
    "sound-hooks@jtcressy-claude-plugins": true
  }
}
```

Restart Claude Code. The plugin will be installed automatically.

To disable all sounds, set `"sound-hooks@jtcressy-claude-plugins": false` in `enabledPlugins`.

## Supported Hook Events

| # | Hook | Description | Sound |
|:-:|------|-------------|-------|
| 1 | `PreToolUse` | Before tool calls | General tool sound |
| 2 | `PermissionRequest` | When permission is requested | Permission alert |
| 3 | `PostToolUse` | After tool calls complete | Completion sound |
| 4 | `PostToolUseFailure` | After tool calls fail | Error sound |
| 5 | `UserPromptSubmit` | When user submits a prompt | Input sound |
| 6 | `Notification` | When notifications are sent | Notification alert |
| 7 | `Stop` | When Claude finishes responding | Stop sound |
| 8 | `SubagentStart` | When subagent tasks start | Agent start sound |
| 9 | `SubagentStop` | When subagent tasks complete | Agent stop sound |
| 10 | `PreCompact` | Before compact operations | Compact warning |
| 11 | `SessionStart` | When a session starts/resumes | Session greeting |
| 12 | `SessionEnd` | When a session ends | Session farewell |
| 13 | `Setup` | When `/setup` runs (legacy, kept for backwards compatibility) | Setup sound |
| 14 | `TeammateIdle` | When a teammate agent idles | Idle alert |
| 15 | `TaskCompleted` | When a background task completes | Task done sound |
| 16 | `ConfigChange` | When config files change | Config change sound |
| 17 | `WorktreeCreate` | When worktrees are created | Worktree sound |
| 18 | `WorktreeRemove` | When worktrees are removed | Worktree sound |
| 19 | `PermissionDenied` | When auto-mode classifier denies a tool call | Denial alert |
| 20 | `StopFailure` | When a turn ends due to an API error | Failure sound |
| 21 | `TaskCreated` | When a task is created via TaskCreate | Task created sound |
| 22 | `PostCompact` | After context compaction completes | Post-compact sound |
| 23 | `InstructionsLoaded` | When CLAUDE.md or `.claude/rules/*.md` is loaded | Instructions loaded sound |
| 24 | `CwdChanged` | When the working directory changes | Cwd changed sound |
| 25 | `FileChanged` | When a watched file changes on disk | File changed sound |
| 26 | `Elicitation` | When an MCP server requests user input mid-tool-call | Elicitation sound |
| 27 | `ElicitationResult` | When a user responds to an MCP elicitation | Elicitation result sound |

### Special Sounds

- **Git commit detection**: When a `Bash` tool runs `git commit`, a special `pretooluse-git-committing` sound plays instead of the default PreToolUse sound.
- **Agent-specific sounds**: 7 agent hook events have distinct sounds in `agent_*` folders, used when the script is invoked from agent frontmatter with `--agent=<name>`.

## Per-Hook Enable/Disable

Individual hooks can be toggled without disabling the entire plugin.

### Configuration Files

| File | Purpose | Committed |
|------|---------|-----------|
| `hooks/config/hooks-config.json` | Default/shared config | Yes |
| `hooks/config/hooks-config.local.json` | Personal overrides | No (gitignored) |

The local config takes precedence over the shared config.

### Example: Disable noisy hooks

Create `hooks/config/hooks-config.local.json` in the plugin's cache directory:

```json
{
  "disablePreToolUseHook": true,
  "disablePostToolUseHook": true,
  "disableLogging": true
}
```

### All Config Keys

```json
{
  "disableLogging": false,
  "disablePreToolUseHook": false,
  "disablePermissionRequestHook": false,
  "disablePermissionDeniedHook": false,
  "disablePostToolUseHook": false,
  "disablePostToolUseFailureHook": false,
  "disableUserPromptSubmitHook": false,
  "disableNotificationHook": false,
  "disableStopHook": false,
  "disableStopFailureHook": false,
  "disableSubagentStartHook": false,
  "disableSubagentStopHook": false,
  "disableTaskCreatedHook": false,
  "disableTaskCompletedHook": false,
  "disablePreCompactHook": false,
  "disablePostCompactHook": false,
  "disableInstructionsLoadedHook": false,
  "disableSessionStartHook": false,
  "disableSessionEndHook": false,
  "disableSetupHook": false,
  "disableTeammateIdleHook": false,
  "disableConfigChangeHook": false,
  "disableCwdChangedHook": false,
  "disableFileChangedHook": false,
  "disableElicitationHook": false,
  "disableElicitationResultHook": false,
  "disableWorktreeCreateHook": false,
  "disableWorktreeRemoveHook": false
}
```

## Prerequisites

- **Python 3**: Required for running the hook script (`python3 --version` to verify)
- **Audio player** (auto-detected):
  - **Linux**: `paplay` (PulseAudio), `aplay` (ALSA), `ffplay`, or `mpg123`
  - **macOS**: `afplay` (built-in)
  - **Windows**: `winsound` (built-in Python module)

## Sound Files

Each hook event has a folder under `hooks/sounds/` containing `.mp3` and `.wav` variants:

```
hooks/sounds/
├── pretooluse/           # includes pretooluse-git-committing variant
├── permissionrequest/
├── permissiondenied/
├── posttooluse/
├── posttoolusefailure/
├── userpromptsubmit/
├── notification/
├── stop/
├── stopfailure/
├── subagentstart/
├── subagentstop/
├── taskcreated/
├── taskcompleted/
├── precompact/
├── postcompact/
├── instructionsloaded/
├── sessionstart/
├── sessionend/
├── setup/
├── teammateidle/
├── configchange/
├── cwdchanged/
├── filechanged/
├── elicitation/
├── elicitationresult/
├── worktreecreate/
├── worktreeremove/
├── agent_pretooluse/     # agent-specific sounds
├── agent_posttooluse/
├── agent_permissionrequest/
├── agent_permissiondenied/
├── agent_posttoolusefailure/
├── agent_stop/
└── agent_subagentstop/
```

## Agent Frontmatter Hooks

To use sound hooks within agent definitions, add hooks to the agent's frontmatter YAML. Agent hooks support 7 events: `PreToolUse`, `PostToolUse`, `PermissionRequest`, `PermissionDenied`, `PostToolUseFailure`, `Stop`, `SubagentStop`.

```yaml
---
name: my-agent
hooks:
  PreToolUse:
    - type: command
      command: python3 ${CLAUDE_PROJECT_DIR}/.claude/hooks/scripts/hooks.py --agent=my-agent
      timeout: 5000
      async: true
  Stop:
    - type: command
      command: python3 ${CLAUDE_PROJECT_DIR}/.claude/hooks/scripts/hooks.py --agent=my-agent
      timeout: 5000
      async: true
---
```

> **Note**: Agent frontmatter hooks reference the script via `${CLAUDE_PROJECT_DIR}`, not `${CLAUDE_PLUGIN_ROOT}`. For agent hooks to work, you'd need to copy the script into your project or reference the plugin cache path.

## License

MIT
