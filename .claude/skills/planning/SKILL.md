---
name: Feature Implementation Planner
description: Automatically create detailed implementation plans whenever the user requests a new feature, mentions adding functionality, discusses building something new, or asks about significant code changes. Triggers on phrases like "implement", "add feature", "create new", "build a", "I want to add", or "let's add".
---

# Feature Implementation Planner

## When to Activate

### Direct Feature Requests
- "I want to add [feature]"
- "Can you implement [feature]"
- "Let's build [feature]"
- "Add [feature] to the app"
- "Create a new [component/screen/feature]"

### Planning Discussions
- "How would we implement [feature]?"
- "What's involved in adding [feature]?"
- "I'm thinking about adding [feature]"
- "We need to [build/create/add] [feature]"

### Significant Changes
- Major refactoring discussions
- Architecture change proposals
- New screen or flow additions
- Multi-file feature implementations

### Avoid False Triggers
**Don't activate for**:
- Simple bug fixes (1-2 file changes)
- Minor tweaks or adjustments
- Documentation updates
- Asking questions about existing features
- Theoretical discussions without implementation intent

## Requirements Gathering

### Step 1: Analyze the Request
Understand what the user wants:
- What is the core feature/functionality?
- What problem does it solve?
- Who is it for (user-facing, developer tool, etc.)?

### Step 2: Ask Clarifying Questions
If not already provided, ask for:

**1. Priority Level**
- Critical (blocks other work)
- High (important, do soon)
- Medium (nice to have)
- Low (future consideration)

**2. Complexity Estimate**
- Simple (1-2 files, straightforward)
- Medium (3-5 files, some integration)
- Complex (5+ files, multiple systems)

**3. Dependencies**
- What existing code does this build on?
- Are there external systems involved?
- Does this require new libraries?

**4. Target Timeline**
- How much time should this take?
- Hours? Days?
- Any deadlines?

**5. Success Criteria**
- How will we know it's done?
- What should it do/not do?
- Any specific requirements?

## Current State Assessment

### Step 3: Examine Existing Code
Before planning, research:

**Check for Existing Patterns**:
- Similar features already implemented?
- Related components in `src/components/`?
- Related hooks in `src/hooks/`?
- Similar services in `src/services/`?

**Review Architecture**:
- Read `CLAUDE.md` for project rules and patterns
- Check file size limits (300 lines max)
- Identify integration points
- Review type definitions in `src/types/index.ts`

**Git History**:
- Any previous work on this feature?
- Related commits or branches?
- Patterns to follow from past features?

**Assess Conflicts**:
- Will this conflict with existing features?
- Any breaking changes required?
- Impact on current user experience?

## Strategy Design

### Step 4: Choose Implementation Approach

**Roll Back + Clean Implementation**:
- When existing code is messy/buggy
- When starting fresh is cleaner
- When current approach is wrong

**Incremental Enhancement**:
- When building on solid foundation
- When existing code is good quality
- When changes are additive

### Step 5: Break Down Into Phases
Divide implementation into logical phases:
- Phase 1: Foundation/Setup
- Phase 2: Core Implementation
- Phase 3: Integration
- Phase 4: Testing & Polish

### Step 6: Risk Assessment
Identify potential blockers:
- Technical challenges
- Integration complexity
- Unclear requirements
- External dependencies

**Mitigation Strategies**:
- How to handle each risk?
- Fallback approaches?
- What could go wrong?

### Step 7: Rollback Plan
If implementation fails or takes too long:
- How to revert cleanly?
- Minimal viable alternative?
- What to ship if pressed for time?

## Generate Implementation Plan

### Step 8: Create Plan Document
Write comprehensive plan to `implementation-plans/YYYY-MM-DD-feature-name-plan.md`

### Plan Structure

```markdown
# [Feature Name] Implementation Plan

**Created**: YYYY-MM-DD
**Priority**: High/Medium/Low
**Complexity**: Simple/Medium/Complex
**Estimated Time**: X hours

## Overview
Brief description (2-3 sentences):
- What we're building
- Why we're building it
- Value proposition

## Current State Analysis

### Problem
What problem are we solving?

### Current Implementation
What exists now?

### Gap Analysis
What's missing or needs to change?

## Strategy: [Chosen Approach]

### Why This Approach
Justification for roll back vs incremental

## Implementation Phases

### Phase 1: [Foundation/Setup] (X minutes)
- [ ] Specific task with file reference
- [ ] Another specific task
- [ ] Third task with time estimate

**Files to Modify/Create**:
- `path/to/file.ts` - What changes
- `path/to/new-file.ts` - Purpose

### Phase 2: [Core Implementation] (X minutes)
- [ ] Implementation task 1
- [ ] Implementation task 2
- [ ] Integration task

**Files to Modify/Create**:
- `path/to/component.tsx` - What changes

### Phase 3: [Testing & Polish] (X minutes)
- [ ] Test scenarios
- [ ] Error handling
- [ ] Edge cases
- [ ] Documentation updates

**Files to Modify/Create**:
- `path/to/test.ts` - Tests to add

## Architecture Compliance

### File Size Check
- Ensure no file exceeds 300 lines
- Extract to hooks if logic >100 lines

### Type Safety
- Use interfaces from `src/types/index.ts`
- Zero `any` types
- Proper TypeScript everywhere

### Component Reuse
- Checked `src/components/` for existing components
- Reusing: [list components]
- Creating new: [justify why needed]

### Service Layer
- All API calls through `src/services/api.ts`
- No direct fetch/axios in components

## Implementation Timeline

### Hour 1: [Phase Name]
- [ ] Specific task 1
- [ ] Specific task 2

### Hour 2: [Phase Name]
- [ ] Specific task 3
- [ ] Specific task 4

## Success Criteria

1. ✅ Criterion 1 (measurable)
2. ✅ Criterion 2 (measurable)
3. ✅ Criterion 3 (measurable)

## Risk Mitigation

### Time Risk
**Risk**: Might take longer than estimated
**Mitigation**: [strategy]

### Technical Risk
**Risk**: [specific technical challenge]
**Mitigation**: [strategy]

### Integration Risk
**Risk**: [integration challenge]
**Mitigation**: [strategy]

## Rollback Plan

If implementation takes too long or fails:

1. **Immediate Rollback**: [steps to revert]
2. **Minimal Viable Alternative**: [simpler version]
3. **Time-Pressed Shipping**: [what to ship if deadline hits]

## Files to Modify/Create

### New Files
- `path/to/new/file.ts` - Purpose
- `path/to/another/file.tsx` - Purpose

### Modified Files
- `path/to/existing/file.ts` - What changes
- `path/to/other/file.tsx` - What changes

### Removed/Deprecated
- `path/to/old/file.ts` - Why removing

## Dependencies

### External Libraries
- None needed (or list if required)

### Internal Dependencies
- Depends on: [existing services/components]
- Blocks: [what depends on this]

## Next Steps

1. Review and approve this plan
2. Create feature branch (if appropriate)
3. Begin Phase 1: [name]
```

## Present Plan to User

### Step 9: Show Plan and Get Approval
```
I've created a detailed implementation plan for [feature name]:

📄 Plan saved to: implementation-plans/YYYY-MM-DD-feature-name-plan.md

## Quick Summary:
- **Estimated Time**: X hours
- **Complexity**: Medium
- **Phases**: 3 phases
- **Files to Modify**: 5 files
- **New Files**: 2 files

## Key Points:
- Phase 1: Foundation (30 min) - Set up data models and types
- Phase 2: Core Implementation (1 hour) - Build main feature
- Phase 3: Testing (30 min) - Error handling and edge cases

## Risks:
- [Main risk and mitigation]

Should I proceed with implementation starting with Phase 1?
```

### Step 10: Ready for Implementation
After approval:
- Suggest creating feature branch if appropriate
- Reference plan file location
- Start with Phase 1, Task 1
- Track progress with todo list

## Integration with Other Skills

### Before Implementation Starts
- Planning Skill creates the plan
- User approves
- Implementation begins

### During Implementation
- review-orchestrator reviews code changes
- commit-enforcer ensures regular commits
- storage-safety runs if storage involved

### After Implementation
- workflow-optimizer may suggest plan improvements
- Lessons learned fed back to planning process

## Examples

### Example 1: New Feature Request
```
User: "I want to add a feature for users to share their workout plans"

Planning Skill activates:
Me: "I'll create an implementation plan for workout plan sharing. Let me ask
a few questions first:

1. Priority: Is this high priority or nice-to-have?
2. Sharing method: Social media, direct link, in-app sharing?
3. Timeline: How soon do you need this?
4. Success criteria: What should users be able to do?

[After user answers]

I've created a comprehensive plan. Let me examine existing code for similar
patterns...

[Reads CLAUDE.md, checks components, reviews architecture]

📄 Plan created: implementation-plans/2025-10-18-workout-sharing-plan.md

Summary:
- 3 phases, ~2 hours total
- Reusing existing ShareButton component
- Adding shareWorkout method to api.ts
- New WorkoutShareModal component

Should I proceed with Phase 1?"
```

### Example 2: Avoiding False Trigger
```
User: "What's the purpose of the useWorkoutPlan hook?"

Planning Skill does NOT activate:
(This is a question about existing code, not a feature request)

Me: "The useWorkoutPlan hook manages workout plan state and provides methods
for loading, updating, and tracking workout plans. Here's how it works..."
```

### Example 3: Major Refactoring
```
User: "We need to refactor the chat system to support multi-turn conversations"

Planning Skill activates:
Me: "This is a significant architectural change. Let me create a detailed plan.

First, some questions:
1. Should we keep backward compatibility with single-turn chats?
2. Timeline constraints?
3. What's the main driver for this change?

[After user answers]

I'll analyze the current chat implementation...

[Examines ChatScreen, chat context, storage patterns]

This is complex - I recommend a phased rollout. Creating detailed plan...

📄 Plan created: implementation-plans/2025-10-18-multi-turn-chat-refactor-plan.md

This is a Complex, 4-hour effort with:
- Phase 1: Extend data models
- Phase 2: Update storage layer (with safety pre-checks)
- Phase 3: Modify ChatScreen
- Phase 4: Migration strategy for existing users

Significant risks identified with mitigation strategies included.

Should I proceed or would you like to review the plan first?"
```

## Quality Guidelines

### Make Plans Actionable
- Every task should be specific and doable
- Include file paths and exact changes
- Provide time estimates for each phase
- Clear success criteria

### Respect Project Rules
- 300 line file limit
- Type safety requirements
- Component reuse principles
- Service layer patterns

### Be Realistic
- Don't underestimate complexity
- Account for integration challenges
- Include buffer time
- Have rollback plans

### Save Plans for Reference
- All plans go in `implementation-plans/`
- Use consistent naming: `YYYY-MM-DD-feature-name-plan.md`
- Plans serve as documentation
- Reference plans in commits

## Success Criteria

This Skill is working well when:
- No significant features start without a plan
- Plans catch architectural issues before coding
- Time estimates are accurate
- Users feel confident approving plans
- Plans serve as useful implementation guides
- Rollback plans actually work when needed
