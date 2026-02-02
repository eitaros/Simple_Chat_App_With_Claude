# Parallel Task Analysis Guidelines

## Purpose
Determine which tasks can be developed independently and concurrently, enabling efficient parallel development.

## Criteria for Parallel Tasks

A task can be marked as parallel `(P)` if it meets **ALL** of these criteria:

### 1. No Implementation Dependencies
- Does not require code, interfaces, or data structures from other pending tasks
- Can be implemented using only:
  - Established interfaces/contracts (already defined in design)
  - External libraries and frameworks
  - Standard language features

### 2. No Sequential Requirements
- Does not need to wait for another task to complete first
- Does not depend on infrastructure or setup from other tasks
- Can start immediately once approved

### 3. Independent Testing
- Can be tested in isolation or with mocks
- Does not require integration with pending features
- Test environment can be set up independently

### 4. Clear Interface Boundaries
- Has well-defined interfaces documented in design
- Integration points are specified via contracts
- Can develop against mock implementations of dependencies

## Common Parallel Scenarios

### ✅ Can Be Parallel

1. **Independent UI Components**
   - Login page and User list component (different screens)
   - Different React components with no shared state

2. **Separate Backend Modules**
   - MessageStore and UserStore (independent data structures)
   - Different event handlers with defined contracts

3. **Infrastructure Setup Tasks**
   - Frontend project setup and Backend project setup
   - Different technology stack configurations

4. **Independent Features**
   - User authentication and Message history display (separate flows)
   - Features with no shared state or logic

### ❌ Cannot Be Parallel

1. **Sequential Dependencies**
   - Socket.IO setup must complete before event handlers
   - React hooks must be defined before components use them
   - Data stores must exist before handlers access them

2. **Shared State or Resources**
   - Two handlers modifying the same store
   - Components sharing the same state management

3. **Integration Requirements**
   - Frontend components depending on backend API completion
   - Testing requiring full system integration

## Marking Tasks as Parallel

### Format
Add `(P)` to the task title:
```markdown
### 2. ユーザーストアの実装 (P)
```

### Guidelines
- **Conservative approach**: When in doubt, don't mark as parallel
- **Review dependencies**: Check design document for integration points
- **Consider team workflow**: Parallel tasks should enable concurrent work by different developers

### Sequential Mode
When `--sequential` flag is used:
- Omit ALL `(P)` markers
- Tasks will be executed in strict order
- Use for single-developer workflow or when dependencies are uncertain

## Decision Matrix

| Scenario | Parallel? | Rationale |
|----------|-----------|-----------|
| Frontend project setup + Backend project setup | ✅ Yes | Independent tech stacks, no shared code |
| MessageStore + UserStore implementation | ✅ Yes | Independent data structures, defined interfaces |
| Socket.IO setup + Event handlers | ❌ No | Handlers depend on Socket.IO server instance |
| Login UI + Chat UI | ⚠️ Maybe | Only if Socket.IO client hook is already implemented |
| Unit tests + Integration tests | ❌ No | Integration tests require components to exist |
| AuthHandler + MessageHandler | ✅ Yes | Independent event handlers with defined contracts |
| Message Input + Message List | ⚠️ Maybe | Only if shared state management is already defined |

## Validation Checklist

Before marking a task as parallel, verify:

- [ ] Task has no code dependencies on other pending tasks
- [ ] All required interfaces are already defined in design
- [ ] Task can be implemented and tested independently
- [ ] Integration points are clear and documented
- [ ] No shared mutable state with other parallel tasks
- [ ] Task does not require infrastructure from other pending tasks

## Example Analysis

### Example 1: Backend Stores

**Task**: Implement MessageStore and UserStore

**Analysis**:
- ✅ Independent data structures
- ✅ Interfaces defined in design
- ✅ No shared state
- ✅ Can test in isolation

**Decision**: Mark both as parallel `(P)`

---

### Example 2: Frontend Components

**Task**: Implement LoginPage and ChatPage

**Analysis**:
- ❌ Both need Socket.IO client hook
- ❌ ChatPage depends on authentication state from LoginPage
- ⚠️ Shared routing and state management

**Decision**:
- LoginPage: Not parallel (establishes auth pattern)
- ChatPage: Not parallel (depends on LoginPage completion)

---

### Example 3: Event Handlers

**Task**: Implement AuthHandler and MessageHandler

**Analysis**:
- ✅ Independent event handlers
- ✅ Socket.IO server interface defined
- ✅ Separate store dependencies (UserStore vs MessageStore)
- ✅ Can test with mocks

**Decision**: Mark both as parallel `(P)` if stores are already implemented

## Edge Cases

### Case 1: "Almost Parallel" Tasks
Tasks that are nearly independent but have a small dependency:
- **Approach**: Make the dependent task sequential
- **Example**: UserList component needs User type definition from backend
  - Solution: Define shared types first, then parallelize components

### Case 2: Optional Parallelization
Tasks that can be parallel but might cause merge conflicts:
- **Approach**: Consider team size and coordination overhead
- **Example**: Two developers modifying the same file
  - Small team: Sequential
  - Large team with code review: Parallel

### Case 3: Testing Tasks
Testing tasks are generally NOT parallel with implementation:
- **Exception**: Test infrastructure setup can be parallel with implementation
- **Rule**: Unit tests for a component cannot be parallel with implementing that component

## Summary

**Default stance**: Tasks are sequential unless proven parallel

**Mark as parallel only when**:
- Independence is clear and verified
- Interfaces are already defined
- No risk of integration conflicts
- Team can actually work in parallel

**When in sequential mode**:
- Remove all `(P)` markers
- Tasks execute in strict order
- Simplifies workflow for solo developers
