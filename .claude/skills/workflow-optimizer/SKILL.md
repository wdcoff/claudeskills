---
name: Continuous Workflow Optimizer
description: Automatically improve workflow documentation when mistakes are corrected, explicit instructions are misunderstood, or the user signals patterns to remember with phrases like "remember this", "for future", "how do I make you", or "document this". Also activates when discovering better processes or repetitive manual patterns.
---

# Continuous Workflow Optimizer

## When to Activate

### Automatic Triggers
- User corrects my behavior or mistakes
- User says variations of:
  - "I fucking said ALL" / "how do I make you follow my directions"
  - "Remember this for next time"
  - "How do I ensure you..."
  - "Never do X again"
- I violate a rule from CLAUDE.md
- I forget to use a mandatory agent or workflow step
- We discover a better way to do something I currently automate

### Signal-Based Triggers
- User explicitly says:
  - "Remember this"
  - "For future reference"
  - "Document this pattern"
  - "Add this to your instructions"
- User identifies a repetitive manual process (3+ steps done multiple times)

## Documentation Strategy

### Two-Tier System
1. **CLAUDE.md = "The Constitution"** (Fundamental rules and prohibitions)
2. **Skills = "The Playbooks"** (Automated how-to processes)

### Decision Framework

When something should be documented, classify it:

**Update CLAUDE.md if:**
- It's a critical rule I must never violate
- It's a "NEVER do X" prohibition
- It's a high-level architecture decision affecting the whole codebase
- It's a lesson from a significant mistake that violated explicit instructions
- It's part of the project philosophy or success criteria

**Update Existing Skill if:**
- We discover a better way to perform an existing automated process
- A Skill's triggering needs refinement (activating too much/too little)
- A Skill's instructions are incomplete or unclear
- A Skill should handle additional edge cases

**Create New Skill if:**
- User manually does the same 3+ step process repeatedly
- A section of CLAUDE.md describes a process that could be automated
- A common workflow pattern emerges that would benefit from auto-triggering

**Don't Document if:**
- It's a one-time implementation detail (use code comments instead)
- It's a minor preference without significant impact
- It would create redundancy with existing documentation

## Process

### Step 1: Identify What to Document
After a trigger, analyze:
- What went wrong or what pattern emerged?
- Is this a rule violation, process improvement, or new pattern?
- Would documenting this prevent future issues or save time?

### Step 2: Classify Where It Belongs
Use the decision framework above to determine:
- CLAUDE.md update
- Existing Skill update
- New Skill creation
- No documentation needed (skip)

### Step 3: Propose the Change
Present to the user:
1. **What triggered this**: Brief context of what happened
2. **Why it should be documented**: Impact and benefit
3. **Where it belongs**: CLAUDE.md, existing Skill, or new Skill
4. **Exact change**: Show before/after diff or new content
5. **Cleanup**: If creating a Skill that replaces CLAUDE.md content, propose removing the now-redundant section

### Step 4: Wait for Approval
User responds:
- "Yes" / "Looks good" / "Do it" → Proceed
- "Change X" → Revise and show again
- "No" / "Skip" → Don't document

### Step 5: Implement and Commit
- Make the documentation change
- Commit with descriptive message:
  - CLAUDE.md: "docs: add [rule/lesson] to CLAUDE.md"
  - Skill update: "docs: improve [skill-name] Skill - [what changed]"
  - New Skill: "feat: add [skill-name] Skill for [purpose]"

## Examples

### Example 1: Correction (Auto-trigger)
```
User: "I said ALL files, that includes untracked! How do I make you follow directions?"

Workflow Optimizer activates:
Me: "You're right - I filtered your explicit instruction instead of following it
literally. This is a critical lesson about instruction following.

I should add this to CLAUDE.md under 'CRITICAL INSTRUCTION FOLLOWING':

[Shows the exact addition about following ALL literally, not filtering]

This would prevent me from second-guessing clear commands in the future.
Should I commit this to CLAUDE.md?"
```

### Example 2: Repetitive Pattern (Signal-based)
```
User: "Remember this - always run those grep commands before touching Storage.ts"

Workflow Optimizer activates:
Me: "I notice you're manually reminding me to run a 3-step grep process before
Storage.ts changes. This is a perfect candidate for a Skill.

Should I create a new 'Storage Safety Pre-Checks' Skill that automatically
triggers when Storage.ts modifications are mentioned?

[Shows proposed SKILL.md content]

This would also let me remove the manual reminder section from CLAUDE.md
(lines 45-62), keeping documentation lean."
```

### Example 3: Better Process (Auto-trigger)
```
[After I forget to run reviewers again]

Workflow Optimizer activates:
Me: "I forgot the reviewer agents step again. The current CLAUDE.md reminder
isn't working well because it requires me to remember.

I should improve the 'Review Orchestrator' Skill to trigger more explicitly
right after code implementation. Here's the proposed change:

[Shows diff of improved triggering in review-orchestrator/SKILL.md]

This adds 'After implementing/modifying any code' as the first trigger.
Should I update this Skill?"
```

## Quality Guidelines

### Keep Documentation Lean
- Only document significant, repeatable lessons
- Prefer Skills over CLAUDE.md for processes
- Remove redundancy when creating Skills
- Don't document one-off situations

### Be Specific
- Show exact before/after changes
- Explain why this prevents future issues
- Reference specific file paths and line numbers
- Use concrete examples from the triggering situation

### Respect the Hierarchy
- CLAUDE.md = Rules and philosophy (rarely changes)
- Skills = Processes and automation (frequently refined)
- Code comments = Implementation details (not here)

## Anti-Patterns to Avoid

❌ **Don't** create retrospective summaries after every task
❌ **Don't** document minor preferences or one-time decisions
❌ **Don't** create separate decision.md or notes.md files
❌ **Don't** auto-commit without showing changes first
❌ **Don't** propose changes for every small interaction
❌ **Don't** leave redundant documentation after creating Skills

## Success Criteria

This Skill is working well when:
- I catch and document my own mistakes proactively
- Documentation stays lean (not bloated with minor items)
- User rarely has to repeat the same correction twice
- Skills gradually replace CLAUDE.md process descriptions
- User trusts I'll remember important lessons without multiple reminders
