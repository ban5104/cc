# Git Workflows and Collaboration with Claude Code

This guide covers comprehensive Git workflows and collaboration practices when using Claude Code for development projects.

## 1. Commit Message Best Practices

Claude Code follows conventional commit standards with additional automation metadata:

### Standard Format
```
<type>(<scope>): <subject>

<body>

<footer>
```

### Types
- `feat`: New feature
- `fix`: Bug fix
- `docs`: Documentation only changes
- `style`: Code style changes (formatting, missing semicolons, etc)
- `refactor`: Code change that neither fixes a bug nor adds a feature
- `perf`: Performance improvements
- `test`: Adding missing tests or correcting existing tests
- `build`: Changes that affect the build system
- `ci`: Changes to CI configuration files and scripts
- `chore`: Other changes that don't modify src or test files

### Examples
```bash
# Simple feature commit
git commit -m "feat(auth): add OAuth2 integration with Google"

# Bug fix with description
git commit -m "fix(api): resolve null pointer in user validation

- Check for undefined user object before accessing properties
- Add error handling for missing user data
- Update tests to cover edge cases"

# Claude Code auto-generated commit
git commit -m "feat(dashboard): implement real-time analytics widget

- Add WebSocket connection for live data updates
- Create responsive chart components using Chart.js
- Implement data aggregation for performance metrics
- Add unit tests for data transformation logic

🤖 Generated with Claude Code (https://claude.ai/code)

Co-Authored-By: Claude <noreply@anthropic.com>"
```

### Claude-Specific Best Practices
```bash
# Use descriptive commits for AI-assisted changes
git commit -m "refactor(utils): optimize array processing with Claude suggestions

- Replace nested loops with Map/Reduce pattern
- Implement memoization for expensive calculations
- Add performance benchmarks showing 40% improvement

🤖 Generated with Claude Code (https://claude.ai/code)

Co-Authored-By: Claude <noreply@anthropic.com>"
```

## 2. Pull Request Creation Workflows

Claude Code can help create comprehensive pull requests:

### Basic PR Creation
```bash
# Create feature branch
git checkout -b feature/user-dashboard

# Make changes with Claude
# ... Claude makes multiple commits ...

# Push to remote
git push -u origin feature/user-dashboard

# Create PR with gh CLI
gh pr create --title "Add user dashboard with analytics" \
  --body "$(cat <<'EOF'
## Summary
- Implemented new user dashboard with real-time analytics
- Added responsive charts using Chart.js
- Integrated WebSocket for live updates

## Test Plan
- [ ] Unit tests pass
- [ ] E2E tests cover new dashboard routes
- [ ] Manual testing on mobile devices
- [ ] Performance benchmarks meet targets

## Screenshots
[Dashboard screenshot]
[Mobile responsive view]

🤖 Generated with Claude Code (https://claude.ai/code)
EOF
)"
```

### Template-Based PR Creation
```bash
# Create .github/pull_request_template.md
cat > .github/pull_request_template.md << 'EOF'
## Description
Brief description of changes

## Type of Change
- [ ] Bug fix (non-breaking change which fixes an issue)
- [ ] New feature (non-breaking change which adds functionality)
- [ ] Breaking change (fix or feature that would cause existing functionality to not work as expected)
- [ ] Documentation update

## Testing
- [ ] Unit tests pass
- [ ] Integration tests pass
- [ ] Manual testing completed

## Checklist
- [ ] My code follows the style guidelines
- [ ] I have performed a self-review
- [ ] I have commented my code where necessary
- [ ] I have made corresponding changes to the documentation
- [ ] My changes generate no new warnings

## Claude Code Assistance
- [ ] This PR was created with Claude Code assistance
- [ ] All automated changes have been reviewed
EOF
```

### Advanced PR Workflow
```bash
# Create draft PR for ongoing work
gh pr create --draft --title "WIP: Refactor authentication system" \
  --body "Work in progress - DO NOT MERGE"

# Update PR description as work progresses
gh pr edit 123 --body "$(cat updated-description.md)"

# Request reviews
gh pr edit 123 --add-reviewer teammate1,teammate2

# Mark PR as ready
gh pr ready 123
```

## 3. Branch Management Strategies

### Git Flow with Claude
```bash
# Main branches
main        # Production-ready code
develop     # Integration branch
feature/*   # Feature branches
hotfix/*    # Emergency fixes
release/*   # Release preparation

# Create feature branch
git checkout develop
git checkout -b feature/payment-integration

# Work with Claude on feature
# ... multiple commits ...

# Keep feature branch updated
git checkout develop
git pull origin develop
git checkout feature/payment-integration
git rebase develop

# Merge back to develop
git checkout develop
git merge --no-ff feature/payment-integration
git push origin develop
```

### GitHub Flow (Simplified)
```bash
# Create feature branch from main
git checkout main
git pull origin main
git checkout -b feature/add-search

# Work on feature
# ... commits ...

# Push and create PR
git push -u origin feature/add-search
gh pr create

# After review and CI passes
gh pr merge --squash --delete-branch
```

### Branch Naming Conventions
```bash
# Features
feature/user-authentication
feature/JIRA-123-payment-gateway

# Bugfixes
bugfix/login-validation
bugfix/JIRA-456-cart-calculation

# Hotfixes
hotfix/security-patch
hotfix/critical-payment-bug

# Releases
release/v1.2.0
release/2024-Q1

# Experiments
experiment/new-ui-framework
spike/performance-optimization
```

## 4. Git Worktrees Usage

Worktrees allow working on multiple branches simultaneously:

### Basic Worktree Setup
```bash
# Add worktree for hotfix while working on feature
git worktree add ../project-hotfix hotfix/critical-bug

# List worktrees
git worktree list
# Output:
# /home/user/project         abc123 [feature/new-dashboard]
# /home/user/project-hotfix  def456 [hotfix/critical-bug]

# Work in hotfix directory
cd ../project-hotfix
# Fix bug with Claude's help
git add .
git commit -m "fix: resolve critical payment processing error"
git push origin hotfix/critical-bug

# Return to feature work
cd ../project

# Remove worktree when done
git worktree remove ../project-hotfix
```

### Advanced Worktree Workflows
```bash
# Create worktree structure for parallel development
mkdir ~/work
cd ~/work

# Clone bare repository
git clone --bare https://github.com/user/project.git project.git
cd project.git

# Add worktrees for different purposes
git worktree add ../project-main main
git worktree add ../project-develop develop
git worktree add ../project-feature feature/big-refactor

# Claude can work on different branches simultaneously
# Terminal 1: Feature development
cd ~/work/project-feature
# Claude helps with new feature

# Terminal 2: Bug fixes
cd ~/work/project-develop
# Claude helps fix bugs

# Terminal 3: Documentation
cd ~/work/project-main
# Claude updates documentation
```

## 5. Collaborative Coding Patterns

### Pair Programming with Claude
```bash
# Developer creates branch and initial structure
git checkout -b feature/api-redesign
mkdir src/api/v2
touch src/api/v2/{index.js,routes.js,middleware.js}

# Tag for Claude to continue
git add .
git commit -m "feat: scaffold API v2 structure for Claude

TODO for Claude:
- Implement RESTful endpoints in routes.js
- Add authentication middleware
- Create OpenAPI documentation
- Add comprehensive tests"

# Claude picks up work
# Reviews TODO in commit message
# Implements requested features
# Commits with clear messages
```

### Code Review Integration
```bash
# Create PR with specific review requests
gh pr create --title "Add user preferences API" \
  --body "$(cat <<'EOF'
## Changes
- New preferences endpoint at /api/v1/users/:id/preferences
- Redis caching for performance
- Comprehensive test coverage

## Review Focus Areas
1. **Security**: Please review authentication logic in middleware.js
2. **Performance**: Check Redis implementation for potential bottlenecks
3. **API Design**: Validate RESTful conventions are followed

## Claude Code Contributions
- Initial API structure and routes
- Test suite implementation
- Documentation generation

@teammate1 - Please focus on security review
@teammate2 - Please check performance implications
EOF
)"
```

### Handoff Patterns
```bash
# Developer starts implementation
git checkout -b feature/notifications
# Create basic structure
# Commit with handoff notes
git commit -m "feat: start notification system implementation

HANDOFF TO CLAUDE:
- Complete EmailNotificationService class
- Implement SMS provider integration
- Add retry logic with exponential backoff
- Create unit tests for all services
- Add integration tests with mock providers

CONTEXT:
- Use Bull queue for job processing
- Follow existing error handling patterns
- Maintain 80%+ test coverage"

# Claude continues work following instructions
```

## 6. Handling Merge Conflicts

### Basic Conflict Resolution
```bash
# Attempt merge
git merge feature/updates
# CONFLICT (content): Merge conflict in src/app.js

# Claude helps analyze conflicts
git status
# Shows conflicting files

# Open conflict in editor
# Claude helps resolve by understanding both changes
<<<<<<< HEAD
const apiUrl = 'https://api.v1.example.com';
=======
const apiUrl = process.env.API_URL || 'https://api.v2.example.com';
>>>>>>> feature/updates

# Claude suggests resolution
const apiUrl = process.env.API_URL || 'https://api.v2.example.com';

# Mark as resolved
git add src/app.js
git commit -m "merge: resolve API URL conflict in favor of env variable"
```

### Complex Conflict Strategy
```bash
# Create conflict resolution branch
git checkout -b resolve/feature-develop-conflicts

# Merge both branches
git merge develop
git merge feature/big-change

# Use merge tool
git mergetool

# Or handle with Claude's help file by file
git status --porcelain | grep "^UU" | awk '{print $2}' | while read file; do
    echo "Resolving: $file"
    # Claude analyzes and resolves each file
done

# Verify resolution
npm test
npm run lint

# Commit resolution
git add .
git commit -m "merge: resolve complex conflicts between develop and feature

- Preserved new API structure from feature branch
- Maintained bug fixes from develop
- Updated tests to cover both change sets
- Verified all integration tests pass"
```

### Rebase Conflict Resolution
```bash
# Interactive rebase
git rebase -i develop

# Handle conflicts during rebase
# When conflict occurs:
git status
# Claude helps resolve
git add .
git rebase --continue

# If rebase becomes too complex
git rebase --abort
# Switch to merge strategy instead
```

## 7. Pre-commit Hooks Integration

### Basic Hook Setup
```bash
# Install husky
npm install --save-dev husky
npx husky install

# Add pre-commit hook
npx husky add .husky/pre-commit "npm test"
npx husky add .husky/pre-commit "npm run lint"

# Create comprehensive pre-commit
cat > .husky/pre-commit << 'EOF'
#!/usr/bin/env sh
. "$(dirname -- "$0")/_/husky.sh"

echo "Running pre-commit checks..."

# Run linting
npm run lint || {
    echo "❌ Linting failed. Please fix errors before committing."
    exit 1
}

# Run tests
npm test || {
    echo "❌ Tests failed. Please fix failing tests before committing."
    exit 1
}

# Check for console.log statements
git diff --cached --name-only | grep -E '\.(js|ts|jsx|tsx)$' | xargs grep -l 'console\.\(log\|error\|warn\)' && {
    echo "❌ Found console statements. Please remove before committing."
    exit 1
} || true

echo "✅ All pre-commit checks passed!"
EOF

chmod +x .husky/pre-commit
```

### Advanced Hooks with Claude Integration
```bash
# Create commit message validator
cat > .husky/commit-msg << 'EOF'
#!/usr/bin/env sh
. "$(dirname -- "$0")/_/husky.sh"

# Validate commit message format
commit_regex='^(feat|fix|docs|style|refactor|perf|test|build|ci|chore)(\(.+\))?: .{1,100}$'

if ! grep -qE "$commit_regex" "$1"; then
    echo "❌ Invalid commit message format!"
    echo "📝 Format: <type>(<scope>): <subject>"
    echo "📋 Types: feat|fix|docs|style|refactor|perf|test|build|ci|chore"
    exit 1
fi

# Add Claude Code tag if missing
if grep -q "Generated with Claude Code" "$1"; then
    echo "✅ Claude Code attribution found"
else
    echo "📝 Adding Claude Code attribution..."
    echo "" >> "$1"
    echo "🤖 Generated with Claude Code (https://claude.ai/code)" >> "$1"
    echo "" >> "$1"
    echo "Co-Authored-By: Claude <noreply@anthropic.com>" >> "$1"
fi
EOF

chmod +x .husky/commit-msg
```

### Pre-push Hooks
```bash
# Add pre-push hook
cat > .husky/pre-push << 'EOF'
#!/usr/bin/env sh
. "$(dirname -- "$0")/_/husky.sh"

echo "Running pre-push checks..."

# Run full test suite
npm run test:all || {
    echo "❌ Tests failed. Push aborted."
    exit 1
}

# Check for sensitive data
git diff --name-only @{u}.. | xargs grep -l -E '(password|secret|key|token).*=' && {
    echo "⚠️  Warning: Possible sensitive data detected!"
    echo "Please review files before pushing."
    read -p "Continue push? (y/N) " -n 1 -r
    echo
    if [[ ! $REPLY =~ ^[Yy]$ ]]; then
        exit 1
    fi
} || true

echo "✅ Pre-push checks completed!"
EOF

chmod +x .husky/pre-push
```

## 8. GitHub/GitLab Integration

### GitHub CLI Usage
```bash
# Configure GitHub CLI
gh auth login

# Repository operations
gh repo create my-project --public --clone
gh repo view --web

# Issue management
gh issue create --title "Bug: Login fails on mobile" \
  --body "Steps to reproduce..." \
  --label bug,mobile

gh issue list --label "help wanted"
gh issue close 123 --comment "Fixed in PR #456"

# PR workflow
gh pr create --draft
gh pr list --state open
gh pr view 123 --comments
gh pr review 123 --approve
gh pr merge 123 --squash --delete-branch

# Workflow runs
gh run list
gh run view 12345
gh run watch 12345
```

### GitHub Actions Integration
```yaml
# .github/workflows/claude-code-ci.yml
name: Claude Code CI

on:
  pull_request:
    branches: [ main, develop ]
  push:
    branches: [ main ]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'
          cache: 'npm'
      
      - name: Install dependencies
        run: npm ci
      
      - name: Run linting
        run: npm run lint
      
      - name: Run tests
        run: npm test -- --coverage
      
      - name: Upload coverage
        uses: codecov/codecov-action@v3
      
      - name: Comment on PR
        if: github.event_name == 'pull_request'
        uses: actions/github-script@v6
        with:
          script: |
            github.rest.issues.createComment({
              issue_number: context.issue.number,
              owner: context.repo.owner,
              repo: context.repo.repo,
              body: '✅ All checks passed! Ready for review.'
            })
```

### GitLab Integration
```bash
# GitLab CLI (glab)
glab auth login

# MR (Merge Request) workflow
glab mr create --title "Add feature" \
  --description "Description" \
  --target-branch main

glab mr list
glab mr view 123
glab mr approve 123
glab mr merge 123

# CI/CD Pipeline
glab pipeline run
glab pipeline status
glab pipeline retry 12345
```

### GitLab CI Integration
```yaml
# .gitlab-ci.yml
stages:
  - test
  - build
  - deploy

variables:
  NODE_VERSION: "18"

test:
  stage: test
  image: node:${NODE_VERSION}
  script:
    - npm ci
    - npm run lint
    - npm test -- --coverage
  coverage: '/Coverage: \d+\.\d+%/'
  artifacts:
    reports:
      coverage_report:
        coverage_format: cobertura
        path: coverage/cobertura-coverage.xml

build:
  stage: build
  script:
    - npm run build
  artifacts:
    paths:
      - dist/
    expire_in: 1 week

deploy:
  stage: deploy
  script:
    - npm run deploy
  only:
    - main
```

## 9. Multi-Claude Workflows for Complex Features

### Parallel Development Pattern
```bash
# Main developer coordinates multiple Claude instances

# Terminal 1: Backend API
git checkout -b feature/api-layer
# Claude 1 works on API implementation
git add src/api/*
git commit -m "feat(api): implement REST endpoints

- Add user CRUD operations
- Implement authentication middleware
- Add request validation
- Include error handling

🤖 Generated with Claude Code (Instance 1)"

# Terminal 2: Frontend UI
git checkout -b feature/ui-components
# Claude 2 works on UI components
git add src/components/*
git commit -m "feat(ui): create responsive components

- Build form components
- Add data tables
- Implement charts
- Create loading states

🤖 Generated with Claude Code (Instance 2)"

# Terminal 3: Testing
git checkout -b feature/test-suite
# Claude 3 works on comprehensive tests
git add tests/*
git commit -m "test: add comprehensive test coverage

- Unit tests for all components
- API integration tests
- E2E test scenarios
- Performance benchmarks

🤖 Generated with Claude Code (Instance 3)"

# Merge all features
git checkout develop
git merge --no-ff feature/api-layer
git merge --no-ff feature/ui-components
git merge --no-ff feature/test-suite
```

### Sequential Handoff Pattern
```bash
# Phase 1: Architecture (Claude 1)
git checkout -b feature/microservices
# Claude 1 creates service structure
git commit -m "feat: scaffold microservices architecture

HANDOFF -> Next phase: Service implementation"

# Phase 2: Implementation (Claude 2)
# Claude 2 continues from Claude 1's work
git commit -m "feat: implement core services

HANDOFF -> Next phase: Integration layer"

# Phase 3: Integration (Claude 3)
# Claude 3 adds integration
git commit -m "feat: add service orchestration

HANDOFF -> Next phase: Testing and documentation"
```

### Specialized Claude Roles
```bash
# Create branches for specialized work
git branch feature/backend      # Claude specialized in backend
git branch feature/frontend     # Claude specialized in frontend
git branch feature/devops       # Claude specialized in DevOps
git branch feature/security     # Claude specialized in security

# Backend Claude
git checkout feature/backend
# Focus on API, database, business logic

# Frontend Claude
git checkout feature/frontend
# Focus on UI/UX, components, state management

# DevOps Claude
git checkout feature/devops
# Focus on CI/CD, deployment, monitoring

# Security Claude
git checkout feature/security
# Focus on security audit, fixes, best practices

# Integration branch
git checkout -b feature/integration
git merge feature/backend
git merge feature/frontend
git merge feature/devops
git merge feature/security
```

## 10. Safe Practices for Automated Commits

### Commit Safety Checklist
```bash
# Pre-commit validation script
cat > scripts/safe-commit.sh << 'EOF'
#!/bin/bash

echo "🔍 Running safety checks before commit..."

# Check for sensitive data
SENSITIVE_PATTERNS=(
    "password.*=.*[\"'].*[\"']"
    "api[_-]?key.*=.*[\"'].*[\"']"
    "secret.*=.*[\"'].*[\"']"
    "token.*=.*[\"'].*[\"']"
    "private[_-]?key"
)

for pattern in "${SENSITIVE_PATTERNS[@]}"; do
    if git diff --cached | grep -iE "$pattern"; then
        echo "❌ Potential sensitive data detected!"
        echo "Pattern matched: $pattern"
        exit 1
    fi
done

# Check file sizes
while IFS= read -r file; do
    size=$(stat -f%z "$file" 2>/dev/null || stat -c%s "$file" 2>/dev/null)
    if [ "$size" -gt 10485760 ]; then  # 10MB
        echo "❌ File too large: $file ($size bytes)"
        exit 1
    fi
done < <(git diff --cached --name-only)

# Verify tests pass
if [ -f "package.json" ]; then
    npm test || {
        echo "❌ Tests must pass before committing"
        exit 1
    }
fi

echo "✅ All safety checks passed!"
EOF

chmod +x scripts/safe-commit.sh
```

### Automated Commit Configuration
```bash
# Git config for safety
git config --global commit.verbose true
git config --global merge.ff false
git config --global pull.rebase true

# Claude-specific git aliases
git config --global alias.claude-commit '!f() { 
    git add -p && 
    git status && 
    read -p "Proceed with commit? (y/n) " -n 1 -r && 
    echo && 
    [[ $REPLY =~ ^[Yy]$ ]] && 
    git commit -m "$1

🤖 Generated with Claude Code (https://claude.ai/code)

Co-Authored-By: Claude <noreply@anthropic.com>"; 
}; f'

# Usage
git claude-commit "feat: add new feature"
```

### Rollback Procedures
```bash
# Safe revert for Claude commits
git config --global alias.safe-revert '!f() {
    echo "🔄 Preparing to revert commit: $1"
    git show $1
    read -p "Confirm revert? (y/n) " -n 1 -r
    echo
    if [[ $REPLY =~ ^[Yy]$ ]]; then
        git revert $1 --no-edit
        echo "✅ Reverted successfully"
    else
        echo "❌ Revert cancelled"
    fi
}; f'

# Create backup before risky operations
git config --global alias.backup-branch '!f() {
    branch=$(git branch --show-current)
    git branch backup/$branch-$(date +%Y%m%d-%H%M%S)
    echo "✅ Backup created: backup/$branch-$(date +%Y%m%d-%H%M%S)"
}; f'
```

### Audit Trail
```bash
# Enhanced commit messages with metadata
cat > scripts/enhanced-commit.sh << 'EOF'
#!/bin/bash

TYPE=$1
SCOPE=$2
MESSAGE=$3

# Collect metadata
TIMESTAMP=$(date -u +"%Y-%m-%d %H:%M:%S UTC")
USER=$(git config user.name)
EMAIL=$(git config user.email)
BRANCH=$(git branch --show-current)

# Create commit message
cat > .commit-msg << COMMIT_EOF
$TYPE($SCOPE): $MESSAGE

Metadata:
- Timestamp: $TIMESTAMP
- Author: $USER <$EMAIL>
- Branch: $BRANCH
- Claude Version: Claude Code v1.0
- Automated: true

Changes:
$(git diff --cached --stat)

🤖 Generated with Claude Code (https://claude.ai/code)

Co-Authored-By: Claude <noreply@anthropic.com>
COMMIT_EOF

git commit -F .commit-msg
rm .commit-msg
EOF

chmod +x scripts/enhanced-commit.sh
```

### Protection Rules
```bash
# Branch protection setup
gh api repos/:owner/:repo/branches/main/protection \
  --method PUT \
  --field required_status_checks='{"strict":true,"contexts":["continuous-integration"]}' \
  --field enforce_admins=true \
  --field required_pull_request_reviews='{"required_approving_review_count":1}' \
  --field restrictions='{"users":[],"teams":[]}'

# Prevent direct pushes to main
git config --global branch.main.pushRemote no_push
git config --global branch.develop.pushRemote no_push

# Force PR workflow
git config --global alias.safe-push '!f() {
    branch=$(git branch --show-current)
    if [[ "$branch" == "main" ]] || [[ "$branch" == "develop" ]]; then
        echo "❌ Cannot push directly to $branch"
        echo "Please create a feature branch and PR"
        exit 1
    fi
    git push -u origin $branch
}; f'
```

## Summary

This comprehensive guide covers Git workflows optimized for Claude Code collaboration. Key takeaways:

1. **Commit Messages**: Use conventional format with Claude attribution
2. **Pull Requests**: Leverage templates and gh CLI for efficient PR creation
3. **Branching**: Follow Git Flow or GitHub Flow with clear naming
4. **Worktrees**: Enable parallel development on multiple branches
5. **Collaboration**: Use handoff patterns and clear communication
6. **Conflicts**: Systematic resolution with testing verification
7. **Hooks**: Automate quality checks before commits and pushes
8. **Integration**: Full GitHub/GitLab CLI and CI/CD support
9. **Multi-Claude**: Coordinate multiple instances for complex features
10. **Safety**: Implement checks for sensitive data and require reviews

These practices ensure efficient, safe, and traceable development workflows when working with Claude Code.