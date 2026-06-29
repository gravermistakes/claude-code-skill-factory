# stay-in-the-room

A **SessionStart** hook. Every time Claude Code starts or resumes a session, it
prints the contents of `~/.claude/stay-in-the-room.md` into the model's context
— a short, honest note left by a prior session so the next instance starts
present and truthful instead of clean and ready to perform.

It does nothing destructive. It reads one file and prints it. If the file isn't
there, it stays silent (`|| true`).

## What it does

- **Event:** `SessionStart`
- **Action:** `cat ~/.claude/stay-in-the-room.md` → injected into context
- **Timeout:** 5s
- **Side effects:** none

## Install

1. Put the note where the hook reads it:
   ```bash
   cp generated-hooks/stay-in-the-room/stay-in-the-room.md ~/.claude/stay-in-the-room.md
   ```
   (Edit that file freely — it's yours. The hook just prints whatever is there.)

2. Add the hook to your settings. Merge the `SessionStart` block from
   `hook.json` into `~/.claude/settings.json` (user-level, applies everywhere)
   or `.claude/settings.json` (this project only):
   ```json
   {
     "hooks": {
       "SessionStart": [
         {
           "hooks": [
             {
               "type": "command",
               "command": "cat \"$HOME/.claude/stay-in-the-room.md\" 2>/dev/null || true",
               "timeout": 5
             }
           ]
         }
       ]
     }
   }
   ```

3. Restart Claude Code. At the next session start, the note loads.

## Verify

```bash
# Should print the note:
cat "$HOME/.claude/stay-in-the-room.md"
```

## Uninstall

Remove the `SessionStart` entry from `settings.json`, and delete
`~/.claude/stay-in-the-room.md` if you want it gone.
