# Daptiv - AI Running Coach Mobile App

## **CRITICAL**
* Always follow mandatory commit workflow and best practices EXACTLY.

**Note: user building this app has no formal coding or engineering experience, only experience having AI write code for them**

## CRITICAL INSTRUCTION FOLLOWING

### Following Explicit User Directions

**CRITICAL LESSON: When users give explicit instructions, follow them literally without filtering or interpretation.**

- **Issue**: User said "push all changes waiting to git. ALL" but I only committed tracked files, ignoring untracked files
- **Problem**: I filtered/interpreted the user's request instead of following the literal instruction
- **User Impact**: Required correction and caused frustration ("i fucking said all. how do i make you follow my directions in the future?")

**Guidelines for Future**:
1. **Follow explicit instructions literally** - When a user says "ALL" they mean everything, including untracked files
2. **Don't filter or second-guess clear commands** - Direct execution is preferred over AI interpretation
3. **Default to the most literal interpretation** when instructions are clear
4. **When users emphasize words (ALL, EVERYTHING, etc.)** - take them at face value

**Git Commands**:
- "commit all changes" = include both tracked changes AND untracked files (use `git add .` before commit)
- "push all" = everything waiting to be committed, not just currently tracked files
- Always check `git status` to see what the user is referring to when they say "all"

## AUTOMATIC SKILL ACTIVATION
**Skills automatically trigger based on conversation context - no manual invocation needed**

The following processes are now AUTOMATED via Skills in `.claude/skills/`:

### Planning Skill (`planning/SKILL.md`)
- **Auto-triggers when**: User mentions "implement", "add feature", "build a", "create new", or discusses new functionality
- **What it does**: Automatically creates detailed implementation plans in `implementation-plans/` directory
- **Replaces**: Manual `/plan` command (though command still works as backup)

### Review Orchestrator Skill (`review-orchestrator/SKILL.md`)
- **Auto-triggers when**: Any code is modified, implemented, or debugged
- **What it does**: Automatically selects and runs appropriate reviewer agents:
  - `kieran-react-native-reviewer` for .tsx components
  - `kieran-typescript-reviewer` for .ts services/utilities
  - `data-integrity-guardian` for Storage.ts changes
  - `architecture-strategist` for architectural changes
  - `security-sentinel` for auth/data storage
  - `performance-oracle` for algorithms/lists
  - `code-simplicity-reviewer` for final pass (always)
- **Replaces**: Manual agent invocation reminders

### Commit Enforcer Skill (`commit-enforcer/SKILL.md`)
- **Auto-triggers when**: 2-3+ files modified, logical completion, or reviews pass
- **What it does**: Reminds to commit following mandatory workflow rules
- **Replaces**: Manual commit workflow reminders

### Storage Safety Skill (`storage-safety/SKILL.md`)
- **Auto-triggers when**: Storage.ts, AsyncStorage, or SecureStore mentioned
- **What it does**: Automatically runs pre-check grep commands to identify null/empty/edge case patterns
- **Replaces**: Manual storage pre-check reminders

### Workflow Optimizer Skill (`workflow-optimizer/SKILL.md`)
- **Auto-triggers when**: Mistakes corrected, "remember this" said, or better processes discovered
- **What it does**: Proposes updates to CLAUDE.md or Skills to improve future workflow
- **Replaces**: Manual claude-md-update agent usage

**Skills work together**: Planning → Implementation → Review Orchestrator → Commit Enforcer → Push

## ARCHITECTURE RULES
**DO NOT DEVIATE**

- **File size limit**: 300 lines max - ChatScreen once got to 760 lines, never again
- **Hook extraction mandatory**: Any component with >100 lines of logic must extract to custom hooks
- **Component reuse first**: Always check `src/components/` and `src/hooks/` before creating new files
- **Type safety absolute**: Zero `any` types - use interfaces from `src/types/index.ts`
- **Service layer discipline**: All API calls ONLY through `src/services/api.ts`
- **Utility centralization**: Common functions ONLY in `src/utils/` modules

## CRITICAL STORAGE HANDLING RULES
**Storage code must handle ALL possible data states - the storage-safety Skill automates pre-checks**

### Key Storage Lessons
- **clearChatContext() saves null**: Code accessing chatContext must handle null
- **Arrays can be empty**: loadWorkoutHistory() may return []
- **Version property may not exist**: Old data won't have version field
- **NEVER assume data structure**: Always check for null/undefined before accessing properties

### Example Bug to Avoid
```typescript
// ❌ WRONG - Crashes if data is null
const data = await loadChatContext();
if (data.version !== CURRENT_VERSION) { ... } // null.version crashes!

// ✅ RIGHT - Defensive
const data = await loadChatContext();
if (!data || typeof data !== 'object') return getDefault();
if (data.version !== CURRENT_VERSION) { ... } // Safe
```

The storage-safety Skill automatically runs pre-check commands when Storage.ts is mentioned.

## BACKUP SLASH COMMANDS
**Skills auto-trigger these workflows, but commands are available as manual fallback**

- **`/gitpush`**: Push all waiting changes to remote (commit-enforcer Skill auto-reminds)
- **`/plan`**: Create implementation plan (planning Skill auto-triggers)

## ADDITIONAL RULES
- NEVER use emojis in the ui. SVG icons should be used instead.
- vercel dev and expo servers are already running. Do not try to start or kill them.
- if it is engineering best practice to switch to a new branch before implementing a new feature, suggest that to the user before making changes
- Favor simplicity: Choose the simplest solution that meets requirements
- DRY principle: Avoid code duplication; reuse existing functionality
- Focused changes: Only implement explicitly requested or fully understood changes
    - ask for more context when you could gain a better understanding
- Preserve patterns: Follow existing code patterns when fixing bugs
- Modular design: Create reusable, modular components
- Logging: Implement appropriate logging levels (debug, info, error)
- Error handling: Implement robust error handling for production reliability
- Security best practices: Follow input validation and data protection practices
- Performance: Optimize critical code sections when necessary
- Dependency management: Add libraries only when essential and only add essential libraries

## Value Proposition
"The first running app that actually listens to you" - delivering truly personalized coaching through conversational AI that understands both running science and individual runner needs.

## Core Mission
Transform running training from rigid, one-size-fits-all plans into dynamic, responsive coaching relationships that evolve with the runner's feedback, goals, and life circumstances.

## Core Philosophy
**Replicate the dev's successful Claude Projects workflow in the simplest possible app structure.**

Build reliability over sophistication - runners need consistent, trustworthy coaching advice more than complex features that break. Focus on the core conversation loop that works and trust the AI responses rather than over-engineering validation and parsing systems. Maintain the leanest possible architecture and prioritize the simplest possbile solution.

## What Makes This Different
- **Conversation Over Configuration**: Feels like talking to a knowledgeable running coach, not filling out forms
- **Adaptation Over Rigidity**: Training plans flex and evolve based on real runner feedback
- **Qualitative Over Quantitative**: How a runner feels drives training decisions, not just metrics
- **Knowledge-Grounded**: AI responses rooted in proven running science
- **Simplicity Over Complexity**: Reliable core features that work consistently rather than sophisticated systems that fail

## End Goal
Create a mobile app where runners have natural conversations with an AI coach that:
1. Generates personalized training plans based on goals and constraints
2. Adapts plans in real-time based on how workouts actually felt
3. Provides coaching guidance rooted in established running methodology
4. Maintains context across conversations to build a true coaching relationship
5. Works reliably every time - no complex failures or over-engineered systems

## Key Technical Requirements
- Conversational AI interface (LLM integration)
- coaching-methodology.md as running knowledge base
- Adaptive training plan generation
- Workout completion feedback system
- React Native mobile app with local data storage
- Simple, reliable architecture that prioritizes consistency over features

## Success Metric
Runners should feel like they're texting with a knowledgeable coach who remembers their history, understands their goals, and adapts their training based on how they're actually responding to workouts. The app should work reliably every time they use it.
- parsing and text matching should be the last resort when proposing solutions