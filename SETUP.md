# Black Belt Dashboard — New Machine Setup

## Git repo
```bash
git clone https://github.com/ScollyNZ/karate_preparation.git
cd karate_preparation
```

Set remote with PAT (rotate PAT first — old one may be compromised):
```bash
git remote set-url origin https://ScollyNZ:YOUR_PAT@github.com/ScollyNZ/karate_preparation.git
```

## Cowork / Claude plugins required
- **Garmin Connect MCP** — install from plugin marketplace, log in to Garmin account

## Scheduled tasks to recreate in Cowork
Both task SKILL.md files are in `scheduled_tasks/` in this repo.

1. **blackbelt-daily-refresh** — 7:00am daily  
   Fetches Garmin data, reads fitness_log.md, updates dashboard artifact, commits + pushes.

2. **karate-morning-briefing** — 8:07am daily  
   Full briefing: Garmin fetch, recommendation, dashboard update, log entry, git commit.

Recreate by telling Claude: *"Create a scheduled task at 7am daily using the instructions in scheduled_tasks/blackbelt-daily-refresh.md"*

## GitHub Pages
Dashboard is live at: https://scollyNZ.github.io/karate_preparation  
Source: `docs/index.html` (main branch, /docs folder — set in repo Settings → Pages)

## Cowork artifact
The dashboard artifact (blackbelt-dashboard) must be recreated from `docs/index.html`:  
Tell Claude: *"Create a new artifact called blackbelt-dashboard from the file docs/index.html in the karate_preparation project folder"*

## Key files
| File | Purpose |
|------|---------|
| `docs/index.html` | Source of truth for the dashboard artifact + GitHub Pages |
| `log.json` | Training/nutrition log (Claude appends entries) |
| `Karate Black Belt Preparation/fitness_log.md` | Manual Garmin snapshots and test results |
| `activities/*.json` | Per-activity HR/splits/commentary |
| `context/*.md` | Background context for Claude |
| `scheduled_tasks/*.md` | Scheduled task definitions |

## Git push workaround (FUSE mount lock issue)
The Linux sandbox cannot unlink lock files on the macOS FUSE mount. Use:
```bash
cp -r .git /tmp/karate_git
rm -f /tmp/karate_git/index.lock /tmp/karate_git/HEAD.lock 2>/dev/null
GIT_DIR=/tmp/karate_git GIT_WORK_TREE=$(pwd) git add -A
GIT_DIR=/tmp/karate_git GIT_WORK_TREE=$(pwd) git commit -m "message"
GIT_DIR=/tmp/karate_git GIT_WORK_TREE=$(pwd) git push origin main
```
