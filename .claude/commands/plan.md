# Create Implementation Plan: $ARGUMENTS

## Step 1: Requirements Analysis
Analyze the feature request: $ARGUMENTS

Ask me for the following information if not already provided:
1. **Priority level** (Critical/High/Medium/Low)
2. **Complexity estimate** (Simple/Medium/Complex)
3. **Dependencies** on existing code or external systems
4. **Target timeline** (hours/days)
5. **Success criteria** - what constitutes "done"

## Step 2: Current State Assessment
- Examine relevant existing code files
- Check git history for related previous work
- Identify integration points with existing systems
- Review CLAUDE.md for any related specifications
- Assess potential conflicts with existing features

## Step 3: Strategy Design
Determine the implementation approach:
- **Roll back + Clean Implementation** vs **Incremental Enhancement**
- **Phase breakdown** - logical implementation steps
- **Risk assessment** - potential blockers and mitigation strategies
- **Rollback plan** - what to do if things go wrong

## Step 4: Generate Implementation Plan
Create a comprehensive plan file in `implementation-plans/YYYY-MM-DD-feature-name-plan.md` with:

### Plan Structure:
```markdown
# [Feature Name] Implementation Plan

## Overview
Brief description and value proposition

## Current State Analysis
- **Problem**: What we're solving
- **Current Implementation**: What exists now
- **Gap Analysis**: What's missing

## Strategy: [Chosen Approach]

### Phase 1: [Foundation/Setup] (X minutes)
- [ ] Specific task with file reference
- [ ] Another task with time estimate
**Files**: List files to modify/create

### Phase 2: [Core Implementation] (X minutes)
- [ ] Implementation tasks
- [ ] Integration tasks
**Files**: List files to modify/create

### Phase 3: [Testing & Polish] (X minutes)
- [ ] Testing tasks
- [ ] Error handling
- [ ] Documentation
**Files**: List files to modify/create

## Implementation Timeline
### Hour 1: [Phase Name]
- [ ] Specific actionable items
### Hour 2: [Phase Name]  
- [ ] Specific actionable items

## Success Criteria
1. ✅ Measurable outcome 1
2. ✅ Measurable outcome 2
3. ✅ Measurable outcome 3

## Risk Mitigation
- **Time Risk**: Mitigation strategy
- **Technical Risk**: Mitigation strategy
- **Integration Risk**: Mitigation strategy

## Rollback Plan
If implementation takes too long or fails:
1. Specific rollback steps
2. Minimal viable alternative
3. What to ship if pressed for time

## Files to Modify/Create
### New Files
- `path/to/new/file.ts` - Purpose
### Modified Files  
- `path/to/existing/file.ts` - What changes
### Removed/Deprecated
- `path/to/old/file.ts` - Why removing
```

## Step 5: Implementation Checklist
Generate a focused checklist for immediate execution:
- Priority tasks that must be completed
- Dependencies between tasks
- Testing verification points
- Integration checkpoints

## Step 6: Time & Resource Planning
Provide realistic estimates:
- **Total time**: X hours breakdown
- **Complexity factors**: What might extend timeline
- **Resource requirements**: Any external dependencies
- **Buffer time**: Built-in contingency

## Step 7: Save and Track
- Save the complete plan to `implementation-plans/` directory
- Add timestamp and feature identifier to filename
- Reference the plan file location for easy access
- Suggest next immediate action to begin implementation