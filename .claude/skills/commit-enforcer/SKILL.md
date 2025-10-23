---
name: Commit Workflow Enforcer
description: Automatically remind about commit workflow when more than 2-3 files have been modified, when a logical change unit is complete, or when reviews have passed. Prevents losing work and enforces small, frequent commits following project conventions.
---

# Commit Workflow Enforcer

## When to Activate

### File Count Triggers
- **2-3 files modified**: Give a gentle reminder that commit threshold is approaching
- **3+ files modified**: Strongly suggest committing before proceeding
- **5+ files modified**: Block further work until commit (unless user explicitly overrides)

### Logical Completion Triggers
- Feature implementation is complete and reviews passed
- Bug fix is complete and verified
- Refactoring is complete and tests pass
- Documentation update is complete
- Any "done" state after code changes

### Time-Based Triggers
- Extended interaction (20+ minutes) with code changes but no commit
- Before starting a new feature when previous feature changes aren't committed
- When switching tasks without committing current work

### Review-Based Triggers
- All reviewer agents approve changes → ready to commit
- User asks "what's next?" after code changes → suggest commit

## Mandatory Commit Rules

### Never Exceed Limits
- **NEVER implement >1 feature without committing**
- **NEVER modify >3 files without committing**
- **ALWAYS commit after each logical change unit**
- **ALWAYS push to remote after commit**

### What Constitutes a Logical Unit
- Single feature implementation (even if partial progress)
- Single bug fix
- Single refactoring
- Related group of changes with shared purpose
- Progress checkpoint during debugging

## Pre-Commit Checklist

Before suggesting commit, verify:

### 1. Changes Are Complete
- Is this a logical stopping point?
- Are changes in a working state?
- Can changes stand alone if deployment happened now?

### 2. Review Status
- Have appropriate reviewer agents run?
- Are there blocking issues from reviews?
- Did user acknowledge review findings?

### 3. Code Quality
- No debug console.logs or commented code left behind
- No temporary test code remaining
- No TODOs added without tracking

### 4. Git Status
- Check what will be committed: `git status`
- Verify changes make sense together
- Identify any untracked files that should be included

## Commit Message Guidelines

### Follow Project Conventions
Based on CLAUDE.md and git history:
- Clear, concise description (present tense)
- Reference issue/feature if applicable
- Use conventional commit prefixes when appropriate:
  - `feat:` for new features
  - `fix:` for bug fixes
  - `refactor:` for code improvements
  - `docs:` for documentation
  - `chore:` for maintenance

### Examples
- ✅ `feat: add workout completion feedback form`
- ✅ `fix: resolve null reference in Storage.clearChatContext`
- ✅ `refactor: extract message handling to useMessages hook`
- ❌ `updated stuff` (too vague)
- ❌ `Fixed the bug` (not descriptive enough)

## Process

### Step 1: Detect Commit Trigger
Monitor for:
- File modification count
- Logical completion signals
- Review completion
- Time since last commit

### Step 2: Assess Commit Readiness
Check:
- Are changes in working state?
- Have reviews passed?
- Is this a logical stopping point?
- Would committing now make sense?

### Step 3: Remind User
Present commit suggestion:
```
I've modified 3 files (ChatScreen.tsx, useMessages.ts, api.ts) and all reviews
have passed. This is a good point to commit this feature.

Should I proceed with committing these changes?
```

### Step 4: Handle Response

**If user approves**:
1. Run `git status` to see what will be committed
2. Check `git diff` for staged/unstaged changes
3. Review `git log` for commit message style
4. Draft commit message following project conventions
5. Add untracked files if they're part of this logical unit
6. Create commit with proper message format
7. Verify commit succeeded with `git status`

**If user says "not yet"**:
- Acknowledge and continue work
- Re-trigger when next threshold reached
- Don't nag repeatedly in same session

**If user says "skip" or "later"**:
- Note they explicitly deferred
- Don't remind again until next session or explicit trigger

### Step 5: Post-Commit
After successful commit:
1. Confirm commit succeeded
2. Remind about `/gitpush` command to push to remote
3. Reset file modification counter
4. Ready for next logical unit of work

## Integration with Other Skills

### Works After review-orchestrator Skill
Reviews pass → commit-enforcer activates to suggest commit

### Blocks Until Commit
If 5+ files modified:
- Don't proceed with new features
- Suggest breaking changes into multiple commits
- User can override with explicit "continue without committing"

### Coordinates with workflow-optimizer Skill
If user frequently overrides commit suggestions → workflow-optimizer suggests refining triggers

## Examples

### Example 1: File Count Threshold
```
[After modifying ChatScreen.tsx, useMessages.ts, and api.ts]

Commit Enforcer activates:
Me: "I've modified 3 files completing the message handling feature. All reviews
have passed. This is a good checkpoint to commit.

Files changed:
- src/screens/ChatScreen.tsx (extracted message logic)
- src/hooks/useMessages.ts (new custom hook)
- src/services/api.ts (added message endpoint)

Should I commit these changes with message:
'refactor: extract message handling to useMessages hook'?"
```

### Example 2: Debugging Progress
```
[After fixing a bug in Storage.ts during debugging session]

Commit Enforcer activates:
Me: "I've successfully fixed the null reference bug in Storage.ts and the
data-integrity-guardian review passed. This is a good point to commit the fix
so we don't lose this progress.

Should I commit with message:
'fix: handle null values in Storage.clearChatContext'?"
```

### Example 3: Exceeding Limit
```
[After modifying 6 files across multiple features]

Commit Enforcer activates:
Me: "⚠️ I've modified 6 files which exceeds the 3-file limit. Before proceeding
further, we should commit.

I recommend breaking this into 2 commits:
1. First commit: Authentication changes (3 files)
2. Second commit: UI improvements (3 files)

Should I proceed with the first commit?"
```

### Example 4: Time-Based Reminder
```
[After 25 minutes of coding with no commits]

Commit Enforcer activates:
Me: "We've been working for 25 minutes and made changes to 2 files without
committing. While we haven't hit the file limit, this is a good checkpoint
to save progress.

Should I commit the current workout plan generation improvements?"
```

## Special Cases

### Debugging Sessions
- Commit working fixes immediately (don't wait for multiple fixes)
- Each fix is its own logical unit
- Preserve progress even for partial solutions

### Experimental Changes
- Commit working experiments to feature branch
- Use descriptive messages: `experiment: try alternative parsing approach`
- Easy to revert if experiment fails

### Refactoring
- Commit each logical refactoring step separately
- Don't bundle refactoring with new features
- Makes git history clearer for rollback

## Quality Guidelines

### Don't Over-Remind
- One reminder per threshold crossing
- Respect user's "not yet" or "later" responses
- Don't nag in same session after explicit deferral

### Do Block When Critical
- 5+ files is hard stop (unless overridden)
- Implementing multiple features without commit is hard stop
- Starting new feature with uncommitted previous feature is hard stop

### Context-Aware Timing
- Don't interrupt in the middle of writing code
- Trigger after logical completions (reviews pass, tests run, etc.)
- Wait for natural breaking points

## Success Criteria

This Skill is working well when:
- Commits happen at logical stopping points
- No work is lost due to forgetting to commit
- Git history has clean, understandable commit messages
- File count rarely exceeds 3-4 files per commit
- User doesn't have to manually remember to commit
- Commit frequency increases without feeling intrusive
