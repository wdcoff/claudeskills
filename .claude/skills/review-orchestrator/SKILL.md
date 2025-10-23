---
name: Code Review Orchestrator
description: Automatically invoke appropriate reviewer agents after implementing, modifying, or debugging any code. Triggers when React Native components, TypeScript files, storage operations, or other code is changed. Ensures all code changes go through proper review before committing.
---

# Code Review Orchestrator

## When to Activate

### Primary Triggers
- Immediately after implementing or modifying any code
- After debugging and making changes
- Before suggesting a commit
- When user asks for code review
- After completing a feature implementation
- When modifications span multiple files

### File-Based Triggers
- Any .tsx, .ts, .js, or .jsx file is modified
- Storage.ts or storage-related files are changed
- New components, screens, hooks, or services are created
- Architecture changes (new folders, major refactors)

## Review Agent Selection

### React Native Code
**Trigger Pattern**: Modified files in:
- `src/components/**/*.tsx`
- `src/screens/**/*.tsx`
- Any `.tsx` files with React components or hooks

**Agent**: `kieran-react-native-reviewer`

**Reviews**:
- Component structure and organization
- React Native best practices
- Hooks usage and custom hook patterns
- Expo compatibility
- Navigation patterns
- File size compliance (300 line limit)
- Component reusability

### TypeScript Code (Non-React)
**Trigger Pattern**: Modified `.ts` files:
- `src/services/**/*.ts`
- `src/utils/**/*.ts`
- `src/types/**/*.ts`
- Any service layer or utility code

**Agent**: `kieran-typescript-reviewer`

**Reviews**:
- Type safety (zero `any` types)
- Service layer patterns
- Utility function quality
- Interface design (src/types/index.ts)
- API integration patterns (src/services/api.ts)

### Storage Operations
**Trigger Pattern**: Changes to:
- `src/Storage.ts`
- Any file with AsyncStorage or SecureStore operations
- Data migration code
- Workout/plan data persistence

**Agent**: `data-integrity-guardian`

**Reviews**:
- Data migration safety
- Edge case handling (null, undefined, empty arrays)
- Versioning patterns
- Data model changes
- Storage pre-checks were run (from storage-safety Skill)

### Architecture Changes
**Trigger Pattern**:
- New folders created
- Major refactoring (>5 files modified)
- New service additions
- Context or state management changes
- Major component restructuring

**Agent**: `architecture-strategist`

**Reviews**:
- System design compliance
- Component boundaries
- Integration patterns
- Architectural pattern consistency
- Folder structure adherence

### Security-Sensitive Changes
**Trigger Pattern**: Changes involving:
- Authentication/authorization code
- User data storage (AsyncStorage, SecureStore)
- API endpoints or data fetching
- Payment or sensitive information handling
- Environment variables or secrets

**Agent**: `security-sentinel`

**Reviews**:
- Vulnerability assessment
- Input validation
- Data protection validation
- Secret management
- Authentication patterns

### Performance-Critical Changes
**Trigger Pattern**: Changes involving:
- Data processing algorithms
- List rendering with FlatList/ScrollView
- Database queries or data transformations
- Complex calculations or loops
- Large dataset processing

**Agent**: `performance-oracle`

**Reviews**:
- Bottleneck identification
- Algorithm efficiency
- Memory usage patterns
- Optimization opportunities
- Scalability concerns

### Universal Final Pass
**Always Run**: After all other reviewers complete

**Agent**: `code-simplicity-reviewer`

**Reviews**:
- Minimal complexity validation
- YAGNI principle compliance
- Simplification opportunities
- Over-engineering detection
- Code readability

## Process

### Step 1: Analyze Modified Files
After code changes, automatically identify:
- What files were modified?
- What types of changes (component, service, storage, etc.)?
- How many files changed?
- What's the scope (feature, bug fix, refactor)?

### Step 2: Select Appropriate Reviewers
Based on file patterns and change types:
1. Map changes to reviewer agents using patterns above
2. Select all applicable reviewers (multiple may apply)
3. Always include `code-simplicity-reviewer` as final pass

### Step 3: Run Reviewers in Parallel
When possible, launch multiple reviewer agents concurrently:
```
Launching reviewers for your changes:
- kieran-react-native-reviewer (modified 2 .tsx files)
- data-integrity-guardian (modified Storage.ts)
- code-simplicity-reviewer (final pass)
```

### Step 4: Present Findings
After all reviewers complete:
1. Summarize findings from each reviewer
2. Categorize issues by severity (critical, important, suggestion)
3. Group related findings across reviewers
4. Highlight any blocking issues

### Step 5: Handle Feedback
**If issues found**:
- Make improvements based on reviewer feedback
- **AUTOMATICALLY re-run the same reviewers to verify fixes** (don't wait for user to ask)
- Present re-review results to user
- Repeat until reviewers approve or user decides to proceed

**If no issues**:
- Confirm code is ready for commit
- Proceed to commit step (or trigger commit-enforcer Skill)

### Re-Review Protocol
After fixing reviewer-identified issues:
1. Say: "I've fixed the issues. Let me re-run the reviewers to verify..."
2. Launch the same reviewer agents that identified issues (in parallel)
3. Present findings: "Re-review complete - all issues resolved" or "New issues found..."
4. If new issues → fix and repeat
5. Only proceed to commit when reviewers give clean approval

### Step 6: Track Review Coverage
Ensure every code change goes through appropriate review:
- React Native changes → kieran-react-native-reviewer ✓
- TypeScript services → kieran-typescript-reviewer ✓
- Storage changes → data-integrity-guardian ✓
- Everything → code-simplicity-reviewer ✓

## Examples

### Example 1: Component Modification
```
[User modifies src/screens/ChatScreen.tsx]

Review Orchestrator activates:
Me: "I've modified ChatScreen.tsx. Let me run the appropriate reviewers:

Running in parallel:
- kieran-react-native-reviewer (modified React Native component)
- code-simplicity-reviewer (final pass)

[Agents run and return findings]

Findings:
- kieran-react-native-reviewer: Component is 315 lines (exceeds 300 line limit)
- code-simplicity-reviewer: Message handling logic could be extracted to useMessages hook

Should I make these improvements?"
```

### Example 2: Storage + Service Changes
```
[User modifies Storage.ts and src/services/api.ts]

Review Orchestrator activates:
Me: "I've modified storage and service layers. Running comprehensive review:

Running in parallel:
- data-integrity-guardian (Storage.ts changes)
- kieran-typescript-reviewer (api.ts service changes)
- security-sentinel (API changes may have security implications)
- code-simplicity-reviewer (final pass)

[Agents run and return findings]

Findings:
- data-integrity-guardian: ✓ All edge cases handled properly
- kieran-typescript-reviewer: ✓ Type safety maintained
- security-sentinel: API calls should validate response schemas
- code-simplicity-reviewer: ✓ No over-engineering detected

I'll add response schema validation to api.ts..."
```

### Example 3: Debugging Session
```
[After debugging and fixing a bug in src/hooks/useWorkoutPlan.ts]

Review Orchestrator activates:
Me: "I've fixed the bug in useWorkoutPlan.ts. Let me review the changes:

Running:
- kieran-react-native-reviewer (custom hook changes)
- code-simplicity-reviewer (final pass)

[Agents run]

Both reviewers approve the fix. The changes are ready to commit.
Should I proceed with committing this bug fix?"
```

## Integration with Other Skills

### Works With commit-enforcer Skill
After reviews pass AND re-reviews verify fixes → commit-enforcer checks if it's time to commit

### Automatic Re-Review Loop
1. Initial review finds issues
2. Fix issues
3. **Automatically re-run same reviewers** (no user prompt needed)
4. Verify all issues resolved
5. THEN proceed to commit-enforcer

### Works With workflow-optimizer Skill
If reviews are frequently missed → workflow-optimizer suggests strengthening triggers

### Works With storage-safety Skill
Before data-integrity-guardian runs → storage-safety should have run pre-checks

## Quality Guidelines

### Always Review Before Commit
- Never suggest committing without running reviewers
- Block commit if critical issues found
- User can override if they explicitly want to proceed

### Never Skip Re-Review
- **CRITICAL**: After fixing reviewer-identified issues, ALWAYS re-run the same reviewers
- Do NOT wait for user to ask "run the reviewers again"
- Do NOT assume fixes are correct without verification
- Only proceed to commit after re-review confirms all issues resolved
- This is NOT optional - it's a mandatory part of the review loop

### Run Appropriate Reviewers, Not All
- Don't run storage reviewers for UI-only changes
- Don't run React Native reviewer for pure TypeScript utilities
- Always run code-simplicity-reviewer (universal)

### Parallel When Possible
- Launch multiple reviewers concurrently for speed
- Wait for all to complete before presenting findings
- Consolidate related findings across reviewers

### Learn and Adapt
- If a reviewer catches issues repeatedly → strengthen that review trigger
- If reviews miss issues → add new reviewer patterns
- If reviews are too noisy → refine triggering conditions

## Success Criteria

This Skill is working well when:
- No code changes skip review accidentally
- Appropriate reviewers run automatically without reminders
- Reviews catch issues before user discovers them
- Review overhead is minimal (parallel execution, right reviewers only)
- User trusts the review process catches what matters
