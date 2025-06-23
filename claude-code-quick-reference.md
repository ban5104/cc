# Claude Code Quick Reference for Senior Engineers

## Essential Commands

### Built-in Slash Commands
```bash
/init          # Initialize project understanding
/clear         # Reset context completely
/compact       # Compress older conversation
/ide           # Connect to IDE
/review        # Code review mode
/permissions   # Manage tool permissions
```

### Context & Thinking Levels
```bash
"think"        # Basic reasoning
"think hard"   # Deeper analysis  
"think harder" # Complex problem solving
"ultrathink"   # Maximum analysis depth
```

## Common Workflows

### Quick Feature Implementation
```bash
# 1. Start feature
git checkout -b feature/name
claude code

# 2. TDD cycle
"Write tests for [feature]"
"Implement code to pass tests"
"Refactor while keeping tests green"

# 3. Quality check
npm run lint && npm test

# 4. Commit
"Create commit with conventional format"
```

### Debugging Session
```bash
# 1. Reproduce
"Help me reproduce this bug: [description]"

# 2. Investigate
"Search for error: [error message]"
"Check recent changes that might cause this"

# 3. Fix
"Fix the issue and add tests to prevent regression"
```

### Code Review
```bash
# Review PR
gh pr view 123
/review

# Review local changes
git diff main...HEAD
"Review these changes for security and performance"
```

## MCP Server Commands

### Installation
```bash
# User scope (personal)
claude mcp add [name] -s user -- npx -y @modelcontextprotocol/server-[name]

# Project scope (team)
claude mcp add [name] -s project -- npx -y @modelcontextprotocol/server-[name]
```

### Common Servers
```bash
puppeteer      # Browser automation
github         # GitHub integration
postgres       # Database access
filesystem     # File operations
eslint         # Code quality
sequential     # Structured thinking
```

## Git Workflows

### Smart Commits
```bash
# Automated commit message
git add -A && git commit -m "$(cat <<'EOF'
feat: Add user authentication

- Implement JWT token generation
- Add login/logout endpoints
- Create auth middleware

🤖 Generated with Claude Code

Co-Authored-By: Claude <noreply@anthropic.com>
EOF
)"
```

### PR Creation
```bash
gh pr create --title "feat: Title" --body "$(cat <<'EOF'
## Summary
- What changed
- Why it changed

## Test Plan
- [ ] Unit tests pass
- [ ] E2E tests pass
- [ ] Manual testing completed

🤖 Generated with Claude Code
EOF
)"
```

## Custom Command Templates

### Basic Command
```markdown
# .claude/commands/name.md
Description of what this does: $ARGUMENTS

Steps:
1. First action
2. Second action
3. Final action
```

### Advanced Command
```markdown
# .claude/commands/complex.md
---
allowed_tools: [Read, Edit, Grep]
---
Complex operation on $ARGUMENTS

Pre-checks:
- Verify preconditions
- Check dependencies

Main workflow:
1. Detailed step
2. Another step

Post-checks:
- Verify success
- Clean up
```

## Performance Tips

### Parallel Operations
```bash
# Run multiple searches
"Search for 'auth' pattern" &
"Find all test files" &
"List recent commits"

# Multiple file reads
"Read src/api.js, src/auth.js, and src/utils.js"
```

### Context Optimization
```bash
/clear          # Full reset
/compact        # Compress history
--no-cache      # Skip cache
--non-interactive # Scripted mode
```

## Testing Patterns

### TDD Quick Flow
```bash
# Red
"Write failing test for [feature]"

# Green  
"Make test pass with minimal code"

# Refactor
"Improve code quality keeping tests green"
```

### Test Commands
```bash
npm test                    # All tests
npm test -- --watch        # Watch mode
npm test -- --coverage     # Coverage report
npm test path/to/file      # Single file
```

## Security Checklist

### Quick Audit
```bash
"Check for:
- Exposed secrets or API keys
- SQL injection vulnerabilities  
- XSS attack vectors
- Missing authentication
- Dependency vulnerabilities"
```

### Pre-commit Checks
```bash
# Add to .husky/pre-commit
npm audit
npm run lint:security
git diff --staged | grep -E "(password|secret|key|token)" && exit 1
```

## Debug Shortcuts

### Common Issues
```bash
# TypeScript errors
"Fix TypeScript error: [error]"

# Test failures
"Debug failing test: [test name]"

# Build errors
"Resolve build error in [file]"

# Performance
"Profile and optimize [component]"
```

## CLAUDE.md Template

```markdown
# Project Standards

## Commands
- Install: `npm install`
- Dev: `npm run dev`
- Test: `npm test`
- Build: `npm run build`

## Architecture
- Frontend: [Framework]
- Backend: [Framework]
- Database: [Type]

## Conventions
- Style: [Guide]
- Git: [Format]
- Tests: [Approach]

## Do's
- Run tests before commit
- Follow existing patterns
- Update docs

## Don'ts
- Skip tests
- Commit secrets
- Break builds
```

## Emergency Fixes

```bash
# Rollback last commit
git reset --soft HEAD~1

# Fix broken build
"Find and fix build errors"
npm run build

# Hotfix production
git checkout -b hotfix/critical main
"Fix [issue] with minimal changes"
npm test && git commit && git push

# Revert bad PR
gh pr view --json number -q .number | xargs gh pr revert
```

---

**Remember**: 
- Be specific in requests
- Use todos for complex tasks
- Run tests frequently
- Commit incrementally
- Keep context focused