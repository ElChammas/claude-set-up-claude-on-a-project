What I added:
- CLAUDE.md: brief project overview, commands (npm run dev, npm test, npm run lint), conventions, architecture note, and safety rules.
- .claude/settings.json: allow Bash(npm test:*), ask Bash(git push:*), deny Read(./.env) and Bash(git push --force:*).


What I left out:
- No secrets, no long dumps of logs or one-off instructions. Personal .claude/settings.local.json is ignored.


How I verified:
- ran `claude --version`
- started a fresh Claude session and used `/memory` (CLAUDE.md loaded) and `/permissions` (rules present)
