# GTD System Implementation Plan for Doom Emacs

## Overview
A streamlined GTD (Getting Things Done) workflow optimized for efficiency and scalability, built on Doom Emacs org-mode.

## Core Design Principles
- **No duplication**: Tasks live in one place, agenda views collect them
- **Minimal friction**: Single keystroke for most common actions  
- **Smart defaults**: Sensible assumptions to reduce decision fatigue
- **Progressive complexity**: Start simple, add features as needed

## File Structure

Make sure to check out the content of ~/org/*

```
~/org/ (git repository)
├── gtd/
│   ├── inbox.org         # Universal capture destination
│   ├── projects.org      # Multi-step outcomes
│   ├── next-actions.org  # Standalone actionable tasks
│   ├── someday.org       # Ideas and future possibilities
│   ├── contacts.org      # People and their information
│   └── archive.org       # Completed items (auto-archived)
└── docs/                 # Documentation (separate from GTD)
```

## Task States

### State Definitions
- `TODO` - Task exists but not yet actionable/processed
- `NEXT` - Active task, ready to work on
- `WAITING` - Blocked on external dependency
- `DONE` - Completed task

### State Transition Rules
```
TODO → NEXT      (activate task)
TODO → WAITING   (identify blocker)
NEXT → DONE      (complete task)
NEXT → WAITING   (encounter blocker)
WAITING → NEXT   (blocker resolved)
```
Ideally, changing state should be done by cycling with TAB (or another easy keybind if TAB already assigned)
### Key Constraints
- Only ONE `NEXT` action per project (maintain focus)
- `WAITING` items must be reviewed weekly
- `DONE` items auto-archive after 30 days

## Capture Workflow

### Quick Capture
**Primary Keybind**: `SPC X` (or `SPC o x` if conflicts exist)

**Default Behavior**:
1. Opens minimal capture buffer
2. Type task description
3. Press `C-c C-c` to file to inbox
4. Returns to previous context

### Capture Templates

**Single Universal Template Approach:**
When you hit `SPC X`, you get ONE template that asks you everything:

```org
* %^{Type|TODO|MEETING|NOTE|CONTACT} %^{Description}
%^{Scheduled?}t
:PROPERTIES:
:CREATED: %U
:CATEGORY: %^{Category|work|personal|both}
:CONTEXT: %^{Context|@home|@office|@computer|@phone|@errands}
%^{Additional tags? (comma separated)}
:END:
%?
```

**How it works:**
1. `SPC X` triggers capture
2. First prompt: Choose type (TODO/MEETING/NOTE/CONTACT)
3. Second prompt: Enter description
4. Third prompt: Add scheduled date? (hit enter to skip)
5. Fourth prompt: Category (work/personal/both)
6. Fifth prompt: Context (@office/@home/etc)
7. Sixth prompt: Any additional tags (optional)
8. Files to inbox.org → Process during review

**Special Templates:**
- `SPC X c` - Quick Contact capture (see below)
- `SPC X n` - Quick Note (no prompts, just type)

### Contact Capture Template

**Keybind**: `SPC X c`

```org
* %^{First Name} %^{Last Name}
:PROPERTIES:
:TYPE: contact
:PHONE: %^{Phone Number}
:EMAIL: %^{Email}
:BIRTHDAY: %^{Birthday (YYYY-MM-DD)}
:COMPANY: %^{Company/Organization}
:NOTES: %^{Quick note about this person}
:CREATED: %U
:END:
```

**Contact features:**
- Birthday automatically creates recurring calendar entry
- Phone/email fields for quick reference
- Company field for professional contacts
- Notes for context (how you met, relationship, etc.)
- Files directly to contacts.org (not inbox)

## Processing & Refiling

### Daily Processing (Morning - 5 minutes)
1. Open inbox: `SPC o a i`
2. For each item, press `RET` to open refile interface
3. Use quick keys:
   - `p` - Refile to projects.org
   - `n` - Refile to next-actions.org
   - `s` - Refile to someday.org
   - `d` - Delete (not actionable)
4. Process to zero items

### Weekly Review (Friday - 30 minutes)
```org
* [ ] Process Inbox to zero
* [ ] Review Projects
  - [ ] Ensure each has a NEXT action
  - [ ] Archive completed projects
* [ ] Review WAITING items
  - [ ] Follow up where needed
  - [ ] Convert to NEXT if unblocked
* [ ] Review Someday/Maybe
  - [ ] Move any to active projects
* [ ] Review Calendar
  - [ ] Check next week's commitments
* [ ] Identify 3 MITs (Most Important Tasks) for next week
```

## Context Tags

| Tag | Description | Usage |
|-----|-------------|-------|
| `@office` | Office days (Mon/Wed/Fri) | Tasks requiring office presence |
| `@home` | WFH days (Tue/Thu) | Home-specific tasks |
| `@computer` | Any computer | General computer work |
| `@macbook` | Work laptop specific | Work environment tasks |
| `@ubuntu` | Dev environment | Development tasks |
| `@phone` | Mobile tasks | Calls, mobile apps |
| `@errands` | Outside tasks | Shopping, appointments |

## Agenda Views

### Keybindings
**Access**: `SPC o a` opens agenda dispatcher

| Key | View | Description |
|-----|------|-------------|
| `d` | Daily | Today's scheduled items + deadlines |
| `n` | Next Actions | All NEXT items across system |
| `w` | Weekly | 7-day calendar view |
| `W` | Waiting | All WAITING items |
| `i` | Inbox | Unprocessed captures |
| `p` | Projects | Project overview with progress |
| `r` | Review | Weekly review checklist |

### Custom Agenda View Example
```elisp
;; Today's Focus View
("d" "Today's Focus" 
 ((agenda "" ((org-agenda-span 1)))
  (todo "NEXT" ((org-agenda-overriding-header "Next Actions")))
  (todo "WAITING" ((org-agenda-overriding-header "Waiting On")))))
```

## Project Management

### Project Structure
```org
* Project Name [1/3]
:PROPERTIES:
:CATEGORY: work
:CREATED: [2025-01-09]
:END:

** Outcome
Clear description of done state

** Tasks
*** NEXT First concrete action
*** TODO Second action
*** TODO Third action

** References
- [[file:~/docs/related-doc.org][Related Documentation]]
- Meeting notes from [2025-01-09]
```

### Project Rules
- Must have clear "Outcome" section
- Only one NEXT action at a time
- Progress cookies show completion [1/3]
- Stuck project = no NEXT action (highlighted in review)

## Dashboard Integration

### Doom Dashboard Modification
Add GTD section showing:
```
GTD Overview:
  📥 Inbox (3)         SPC o a i
  ⚡ Next Actions (8)   SPC o a n
  📅 Today             SPC o a d
  📁 Projects (5)      SPC o a p
```

### Quick Stats Display
- Inbox count (should trend to zero)
- Active NEXT actions
- Projects with/without NEXT
- Upcoming deadlines (within 3 days)

## Synchronization Strategy (Detailed)

### Initial Git Setup
```bash
# One-time setup commands:
cd ~
mkdir org
cd org
git init
git add .
git commit -m "Initial GTD setup"

# Create GitHub private repo (via web), then:
git remote add origin git@github.com:yourusername/org-gtd.git
git push -u origin main
```

### Dropbox Symlink Setup
```bash
# Create Dropbox folder if not exists
mkdir -p ~/Dropbox/org

# Move inbox.org to Dropbox and create symlink
mv ~/org/gtd/inbox.org ~/Dropbox/org/inbox.org
ln -s ~/Dropbox/org/inbox.org ~/org/gtd/inbox.org

# Git will track the symlink, not the file content
```

### Daily Sync Workflow

**Morning (on any device):**
```bash
cd ~/org
git pull origin main
# If conflicts, resolve them
# Start working
```

**Throughout the day:**
- Files auto-save every change
- Git auto-commits with timestamp
- Dropbox syncs inbox.org instantly

**End of day:**
```bash
cd ~/org
git push origin main
```

### Auto-commit Configuration
```elisp
;; In config.el - auto-commit on save
(add-hook 'after-save-hook
  (lambda ()
    (when (string-match-p "org/gtd" buffer-file-name)
      (shell-command 
        (format "cd %s && git add . && git commit -m 'Auto-save: %s'"
          (file-name-directory buffer-file-name)
          (format-time-string "%Y-%m-%d %H:%M:%S"))))))
```

### Handling Conflicts

**If git pull shows conflicts:**
1. Check what's conflicting: `git status`
2. Open conflicted file in Emacs
3. Look for conflict markers: `<<<<<<<`
4. Choose which version to keep
5. Remove conflict markers
6. `git add .` and `git commit -m "Resolved conflict"`

**Simple sync script** (save as ~/org/sync.sh):
```bash
#!/bin/bash
cd ~/org

# Stash any uncommitted changes
git stash

# Pull latest
git pull origin main

# Apply stashed changes
git stash pop

# If conflicts, open Emacs to resolve
if [ $? -ne 0 ]; then
    echo "Conflicts detected! Opening Emacs..."
    emacs ~/org/gtd/
fi

# Push changes
git add .
git commit -m "Sync: $(date)"
git push origin main
```

### Mobile Capture Flow
1. Use any org-mode app on phone (Orgzly, beorg, etc.)
2. App saves to Dropbox/org/inbox.org
3. Desktop Emacs sees changes immediately (via symlink)
4. Git commits preserve history
5. Process inbox.org during daily review

## Time Management

### Scheduling vs Deadlines

| Type | Meaning | Usage | Visual |
|------|---------|-------|--------|
| SCHEDULED | "I plan to work on this" | Daily planning | Shows in daily agenda |
| DEADLINE | "Must be done by" | Hard constraints | Warning as date approaches |

### Time Blocking (Future Enhancement)
- Use effort estimates: `:EFFORT: 1h`
- Block calendar time for NEXT actions
- Integration with Google Calendar (both personal/work)

## Summary of Current Status

### What's Working:
1. **~/org/ directory structure created** with all GTD files
2. **Git repository initialized** in ~/org/
3. **Basic documentation created** (PLAN.md and GTD-QUICK-REFERENCE.md)

### What's NOT Working:
1. **No Doom Emacs configuration** - config.el is back to default
2. **No capture templates** configured
3. **No agenda views** configured
4. **No keybindings** set up
5. **No auto-commit** on save
6. **No sync strategy** implemented

### Next Steps Needed:
To implement this GTD system, we need to:
1. Find a keybinding scheme that doesn't conflict with Doom
2. Add the GTD configuration to config.el
3. Test everything works
4. Set up sync (git + optional Dropbox)

## Implementation Status

### ✅ COMPLETED: File Structure Setup
- [x] Created ~/org/ directory
- [x] Initialized git repository in ~/org/
- [x] Created gtd/ subdirectory
- [x] Created all .org files (inbox, projects, next-actions, someday, contacts, archive)
- [x] Created docs/ subdirectory
- [x] Files are ready and working

### ❌ NOT STARTED: Configure Task States
- [ ] Add TODO/NEXT/WAITING/DONE states to config.el
- [ ] Configure state colors and symbols
- [ ] Set up state transition logging
- [ ] Configure automatic timestamps on state changes
- [ ] Test state transitions work correctly

### ❌ NOT STARTED: Setup Capture Templates
- [ ] Configure universal capture template
- [ ] Configure contact capture template
- [ ] Configure quick note template
- [ ] Set inbox.org as default capture target
- [ ] Set contacts.org as contact capture target
- [ ] Test all capture templates work

### ❌ NOT STARTED: Configure Refile Targets
- [ ] Set up refile targets (projects, next-actions, someday)
- [ ] Configure refile quick keys (p, n, s)
- [ ] Set up IDO or Helm completion for refile
- [ ] Configure refile to show full path
- [ ] Test refiling from inbox to all targets

### ❌ NOT STARTED: Setup Agenda Views
- [ ] Configure daily agenda view (d)
- [ ] Configure NEXT actions view (n)
- [ ] Configure weekly view (w)
- [ ] Configure WAITING items view (W)
- [ ] Configure inbox view (i)
- [ ] Configure projects overview (p)
- [ ] Test all agenda views display correctly

### ❌ NOT STARTED: Dashboard Integration
- [ ] Add GTD section to Doom dashboard
- [ ] Display inbox count
- [ ] Add quick links to agenda views
- [ ] Add quick links to main org files
- [ ] Test dashboard loads correctly on startup

### ⚠️ PARTIALLY COMPLETE: Git & Sync Configuration
- [x] Git repository initialized in ~/org/
- [ ] Configure auto-save hooks
- [ ] Set up auto-commit on save
- [ ] Create sync script for push/pull
- [ ] Set up Dropbox symlink for inbox.org
- [ ] Test conflict resolution workflow
- [ ] Document manual sync commands

### ❌ NOT STARTED: Keybinding Configuration
- [ ] Choose keybinding scheme that doesn't conflict with Doom
- [ ] Configure capture keybinding
- [ ] Configure agenda view keybindings
- [ ] Configure file quick-access keybindings
- [ ] Test all keybindings work
- [ ] Document all custom keybindings

**ISSUE DISCOVERED:** 
- Doom Emacs has many existing keybindings that conflict
- Need to find a clean namespace for GTD commands
- Options to consider:
  1. Use a dedicated prefix like `SPC g` for GTD
  2. Use function keys (F5-F8)
  3. Override some less-used Doom bindings
  4. Create a hydra/transient menu

### ❌ NOT STARTED: Testing & Refinement
- [ ] Complete full capture → process → archive cycle
- [ ] Test project with multiple tasks
- [ ] Test WAITING state workflow
- [ ] Test daily and weekly review process
- [ ] Verify sync works across devices
- [ ] Document any issues or adjustments needed

### ✅ COMPLETED: Documentation
- [x] Created GTD-QUICK-REFERENCE.md (but needs updating once implementation is done)
- [x] Created PLAN.md with full system design
- [ ] Update quick reference with actual working keybindings
- [ ] Test and verify all documented workflows

## Success Metrics
- Inbox processed to zero daily
- All projects have NEXT actions
- Weekly review completed consistently
- Capture time < 10 seconds
- No missed deadlines

## Configuration Location
All configuration will be added to:
- `~/.config/doom/config.el` - Main configuration
- `~/.config/doom/packages.el` - Any additional packages
- `~/.config/doom/init.el` - Module activation

## Notes
- Start with minimal configuration
- Add complexity only when needed
- Review and adjust weekly
- Keep documentation updated
