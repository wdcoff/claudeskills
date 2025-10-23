---
name: Storage Safety Pre-Checks
description: Automatically run safety checks and data pattern analysis before any modifications to Storage.ts, AsyncStorage operations, SecureStore changes, or data persistence code. Prevents crashes by ensuring defensive coding for all existing data patterns (null, empty arrays, missing properties).
---

# Storage Safety Pre-Checks

## When to Activate

### File-Based Triggers
- User mentions modifying `Storage.ts`
- Discussion about AsyncStorage operations
- Planning SecureStore changes
- Creating data migrations
- Implementing data persistence

### Keyword Triggers
- "storage" in context of code changes
- "save" or "load" in context of data operations
- "AsyncStorage" or "SecureStore" mentioned
- "data migration" or "versioning" discussed
- "workout data" or "plan data" persistence

### Planning Triggers
- Before implementing features that persist data
- During debugging of storage-related issues
- When planning to modify data models
- When discussing data structure changes

## Critical Pre-Check Commands

**ALWAYS run these THREE commands before ANY Storage.ts modifications:**

### Command 1: Check Null Saves
```bash
rg "save\([^,]+,\s*null\)" --type ts
```
**Purpose**: Identify all places where `null` is explicitly saved
**Why**: New storage code must handle null values defensively

### Command 2: Check Empty/Null Returns
```bash
rg "Promise<.*\[\]>" src/Storage.ts
```
**Purpose**: Find methods that can return empty arrays
**Why**: Code must handle both populated and empty array returns

### Command 3: Check All Edge Case Returns
```bash
rg "return \[\]|return null|return ''" src/Storage.ts
```
**Purpose**: Map all possible edge case return values
**Why**: Ensure new code doesn't assume data is always present or shaped a certain way

## Analysis Process

### Step 1: Run Pre-Check Commands
Execute all three commands and capture results

### Step 2: Analyze Patterns
Document findings:

**Null Patterns**:
- Where is null saved?
- What keys accept null values?
- Example: `clearChatContext()` saves null

**Empty Array Patterns**:
- What methods can return []?
- When do they return empty vs populated?
- Example: `loadWorkoutHistory()` may return []

**Edge Cases**:
- What can be undefined?
- What can be missing properties?
- What data might not have version property?

### Step 3: Assess Impact
For the planned changes:
- Will code handle null values?
- Will code handle empty arrays?
- Will code handle missing properties?
- Will code handle data without version property?
- Are all possible states accounted for?

### Step 4: Present Findings
Show user the analysis:
```
I've run storage pre-checks before modifying Storage.ts:

Found patterns:
- clearChatContext() saves null values (line 45)
- loadWorkoutHistory() can return [] (line 78)
- Some saved data may not have version property

Your planned change to add versioning must account for:
1. Existing null values in storage
2. Empty arrays from past saves
3. Data without version property

I'll add defensive checks for these cases in the implementation.
```

### Step 5: Implement Defensively
Apply findings to implementation:
- Add null checks before object access
- Handle empty array cases
- Provide defaults for missing properties
- Don't assume data structure

## Defensive Coding Rules

### Rule 1: Never Assume Data Structure
❌ **Wrong**:
```typescript
const data = await loadData();
return data.version; // Assumes data is object with version
```

✅ **Right**:
```typescript
const data = await loadData();
if (!data || typeof data !== 'object') return null;
return data.version ?? DEFAULT_VERSION;
```

### Rule 2: Handle All Possible States
Check for:
- `null`
- `undefined`
- Empty arrays `[]`
- Empty objects `{}`
- Missing properties
- Wrong types

### Rule 3: Provide Sensible Defaults
When data is missing or malformed:
- Return safe default values
- Log the issue for debugging
- Don't crash the app

### Rule 4: Validate Before Accessing
```typescript
// Always check existence before accessing nested properties
if (data?.workouts?.length > 0) {
  // Safe to access workouts array
}
```

## Integration with Other Skills

### Before data-integrity-guardian
- Storage-safety runs pre-checks FIRST
- data-integrity-guardian reviews implementation AFTER
- Together they ensure storage reliability

### Coordinates with review-orchestrator
- review-orchestrator calls storage-safety automatically
- When Storage.ts changes detected → storage-safety activates
- Then data-integrity-guardian reviews the implementation

## Real-World Example: The Version Property Bug

### What Happened
```typescript
// Buggy code assumed all data has version property
const data = await loadChatContext();
if (data.version !== CURRENT_VERSION) { // CRASH! data could be null
  migrate(data);
}
```

### The Problem
- `clearChatContext()` saves `null`
- New versioning code assumed data would be an object
- Accessing `null.version` crashes the app

### What Pre-Checks Would Have Revealed
```bash
$ rg "save\([^,]+,\s*null\)" --type ts

Storage.ts:
45: await AsyncStorage.setItem('chatContext', null);
```

**Analysis**: "chatContext can be null, so versioning code must handle null case"

### Defensive Fix
```typescript
const data = await loadChatContext();

// Handle null case defensively
if (!data || typeof data !== 'object') {
  return getDefaultChatContext();
}

// Now safe to check version
if (data.version !== CURRENT_VERSION) {
  return migrate(data);
}

return data;
```

## Examples

### Example 1: Planning Storage Change
```
User: "I want to add versioning to all stored data"

Storage Safety activates:
Me: "Before adding versioning, let me run storage pre-checks to understand
existing data patterns:

[Runs 3 grep commands]

Findings:
- clearChatContext() saves null (chatContext can be null)
- loadWorkoutHistory() returns [] when empty
- Some data structures may not have version property yet

Implementation must:
1. Handle null values (return default or skip versioning)
2. Handle empty arrays (valid state, version as empty)
3. Add version to new data, but not crash on old data without it

Should I proceed with this defensive approach?"
```

### Example 2: Debugging Storage Issue
```
User: "Users are getting crashes when loading workout data"

Storage Safety activates:
Me: "Let me run storage pre-checks to identify potential edge cases:

[Runs 3 grep commands]

Found:
- loadWorkoutData() can return null (line 134)
- Some saved workouts may be [] empty array
- Workout objects may be missing 'completedAt' property

The crash is likely from accessing properties on null/undefined. I'll add
defensive checks for all these cases."
```

### Example 3: Migration Planning
```
User: "We need to migrate old workout format to new structure"

Storage Safety activates:
Me: "Running pre-checks before planning migration:

[Runs 3 grep commands]

Current state:
- Workout data can be null, [], or array of objects
- Old workouts may not have 'intensity' field
- Some users may have never saved workouts (undefined)

Migration strategy:
1. Handle null case → return empty new format
2. Handle [] case → return empty new format
3. For existing workouts → add default 'intensity' if missing
4. Never crash if old data is unexpected shape

This defensive approach ensures migration works for all users."
```

## Quality Guidelines

### Always Run Pre-Checks First
- Never modify Storage.ts without running all 3 commands
- Present findings before implementing
- Get user confirmation on defensive strategy

### Document Edge Cases in Code
Add comments explaining defensive checks:
```typescript
// chatContext can be null (from clearChatContext())
if (!chatContext) return DEFAULT_CONTEXT;

// Workout history can be empty array
if (workouts.length === 0) return [];
```

### Test Edge Cases
Verify implementation handles:
- Null values
- Empty arrays
- Missing properties
- Unexpected types

## Success Criteria

This Skill is working well when:
- No Storage.ts modifications happen without pre-checks
- Storage-related crashes decrease to zero
- All storage code has defensive null/undefined handling
- Data migrations work for all existing user data states
- I automatically remember to check patterns before changing storage
