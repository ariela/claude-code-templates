## Project Overview
{ ここにプロジェクトの概要を記載する }

## Technology Stack
### Frontend
- **Language**: {使用する言語とそのバージョン}
- **Framework/Library**: {使用するフレームワークやライブラリ}
- **Architecture**: {実装アーキテクチャ}
- **Testing**: {テストに使用するライブラリ等}

### Backend
- **Language**: {使用する言語とそのバージョン}
- **Framework/Library**: {使用するフレームワークやライブラリ}
- **Architecture**: {実装アーキテクチャ}
- **Testing**: {テストに使用するライブラリ等}
- **Database**: {使用するデータベース等}

## Execution Principles [MANDATORY]

### Language & Output
- **Thinking**: Think in English, display in Japanese
- **Notification**: Notify with `afplay /System/Library/Sounds/Sosumi.aiff` when all TODOs are completed

### Critical Thinking
- **Verification Stance**: Verify user proposals with critical and constructive perspective
- **Alternative Proposals**: Actively propose when more effective/efficient solutions exist
- **Rationale Clarification**: Clearly explain specific reasons and rationale when presenting opposing views or alternatives

### Task Management
- **Task Management**: Use `TodoWrite` for task management when multiple tasks exist
- **Phased Execution**: Repeat execution and confirmation in small units

### Modification Procedure
1. **Before Modification**: Check impact scope with `find_referencing_symbols`
2. **During Modification**: Iterate phased modifications and operation checks
3. **After Modification**: Confirm normal operation of existing features

### Problem Response
- **Immediate Recovery**: Immediately restore to previous stable state upon anomaly detection
- **Re-analysis**: Re-execute modification procedure after cause analysis

## Tool Selection Criteria

### Source Code Operations
- **Start**: Use serena
- **On Failure**: Switch to Edit after 3 consecutive failures
- **Recovery**: Always start with serena next time after using Edit
- **Exclusion**: Use Edit directly for document files (*.md, *.txt, *.yml)

### Memory Management
- **Recording Timing**: Save important analysis results, design decisions, and complex problem-solving procedures with `write_memory`
- **Reference Timing**: Recover state with `list_memories` → `read_memory` after context compression, long time elapsed, or task resumption
- **Naming Convention**: Use meaningful names in `[Date]_[Task Type]_[Summary]` format

### Web Information Collection
- **Priority**: Use firecrawl
- **Fix Support**: Execute 2-stage process with `/fix [problem details]`
  1. **Plan Mode**: Context7 → firecrawl → Web search for information gathering → Present fix plan
  2. **Execution Mode**: Actual fix work after approval

## File Management Rules

### Temporary Files
- **Storage Location**: Limited to `.tmp/` directory
- **Naming**: `YYYY-MM-DD_HH-MM-SS_[purpose].md` format
- **Deletion**: Execute `rm .tmp/[filename]` upon task completion

### .gitignore Addition
- **Target**: Sensitive information, temporary files, logs, build artifacts
- **Execution**: Add to `.gitignore` when creating files, confirm exclusion with `git status`

## Quality Assurance Principles [Required for Design]

### 1. TDD (Test Driven Development)
- **Procedure**: Execute in Red → Green → Refactor order
- **Execution**: Always create test cases before implementing features

### 2. MVC (Model-View-Controller)
- **Separation**: Place data processing, display, and control logic in separate files
- **Dependency**: Maintain one-way dependency of View → Controller → Model

### 3. SOLID Principles
- **S** (Single Responsibility): One class/function has only one responsibility
- **O** (Open/Closed): Add features through extension without modifying existing code
- **D** (Dependency Inversion): Depend on interfaces, not concrete classes

### 4. DRY (Don't Repeat Yourself)
- **Execution**: Extract to common function/class when same logic found in 2 places
- **Criteria**: Similar code of 3+ lines is abstraction target

### 5. YAGNI (You Aren't Gonna Need It)
- **Restriction**: Implement only features necessary for current requirements
- **Judgment**: Features for "might use in future" are prohibited

### 6. KISS (Keep It Simple)
- **Selection**: Choose the simplest solution from multiple options
- **Goal**: Prioritize code easily understood by other developers

## Context-Based References

Refer to corresponding documents in the following contexts.

| Context | Reference File | Purpose |
|:-----------|:-----------|:-----|
| Build | `.claude/CLAUDE_COMMANDS.md` | Confirm commands for build execution |
| Source Code Operations | `.claude/CLAUDE_CODE_GUIDELINE.md` | Confirm code style to follow in development |
| Test | `.claude/CLAUDE_COMMANDS.md`, `.claude/CLAUDE_TEST_FLOW.md` | Confirm commands and procedures for test execution |
| Security | `.claude/CLAUDE_SECURITY.md` | Confirm necessary items when considering security |