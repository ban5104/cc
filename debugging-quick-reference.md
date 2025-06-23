# Claude Code Debugging Quick Reference

## 🚀 Quick Start Commands

### Initial Project Exploration
```bash
# 1. Understand project structure
LS("/project/root", ["node_modules", ".git", "dist"])
Read("package.json")
Read("README.md")

# 2. Find main entry points
Glob("**/index.{js,ts,jsx,tsx}")
Glob("**/main.{js,ts,py}")
Glob("**/app.{js,ts,py}")

# 3. Understand dependencies
Bash("npm ls --depth=0")
```

### Common Debugging Patterns

#### 🐛 JavaScript/TypeScript Errors

**"Cannot read property 'x' of undefined"**
```bash
# Find the error
Grep("\\.x[^a-zA-Z]", "src/**/*.{js,ts}")

# Fix with optional chaining
Edit(
    file_path="/path/to/file.js",
    old_string="object.x",
    new_string="object?.x",
    replace_all=True
)
```

**"Module not found"**
```bash
# Check if module exists
Bash("npm ls module-name")

# Install if missing
Bash("npm install module-name")

# Check import path
Grep("from.*module-name|require.*module-name")
```

**Async/Promise Issues**
```bash
# Find unhandled promises
Grep("\\.then\\([^)]*\\)(?!.*\\.catch)")

# Find missing awaits
Grep("async.*function|async.*=>")
```

#### 🔍 Performance Issues

**Slow Page Load**
```bash
# Find large bundles
Bash("npm run build -- --stats")

# Find synchronous operations
Grep("readFileSync|execSync|localStorage\\.getItem")

# Find render bottlenecks
Grep("componentDidUpdate|useEffect\\(\\)")
```

**Memory Leaks**
```bash
# Find event listeners without cleanup
Grep("addEventListener(?!.*removeEventListener)")

# Find setInterval without clearInterval
Grep("setInterval(?!.*clearInterval)")

# Find large arrays/objects
Grep("new Array\\(|push\\(.*loop|concat\\(")
```

#### 🔒 Security Issues

**Check for vulnerabilities**
```bash
# Run security audit
Bash("npm audit")

# Find hardcoded secrets
Grep("api_key|password|secret|token", "**/*.{js,env}")

# Find eval usage
Grep("eval\\(|Function\\(|innerHTML")
```

## 📊 Analysis Commands

### Code Quality Checks
```bash
# Run linter
Bash("npx eslint src/ --ext .js,.jsx,.ts,.tsx")

# Check types
Bash("npx tsc --noEmit")

# Find complexity
Grep("if.*if.*if|for.*for|while.*while")
```

### Find Code Smells
```bash
# Long files
Bash("find src -name '*.js' -exec wc -l {} + | sort -nr | head -10")

# Duplicate code
Grep("copy|paste|duplicate|TODO.*refactor")

# Console logs
Grep("console\\.(log|error|warn)")
```

## 🛠️ Fix Common Issues

### React/Vue/Angular Issues

**React Hook Errors**
```bash
# Find hooks in conditions
Grep("if.*use[A-Z]|\\? use[A-Z]")

# Find missing dependencies
Grep("useEffect\\(.*\\[\\]\\)")
```

**State Management Issues**
```bash
# Find direct state mutations
Grep("state\\.|this\\.state.*=")

# Find missing keys in lists
Grep("\\.map\\(.*=>.*<(?!.*key=)")
```

### Database Issues

**N+1 Queries**
```bash
# Find potential N+1
Grep("for.*await.*find|map.*async.*query")

# Find missing indexes
Grep("WHERE|ORDER BY|GROUP BY", "**/*.sql")
```

## 🔧 Quick Fixes

### Add Error Handling
```python
MultiEdit(
    file_path="/src/api/client.js",
    edits=[
        {
            "old_string": "fetch(url)",
            "new_string": "fetch(url).catch(err => console.error('API Error:', err))"
        },
        {
            "old_string": "async function getData() {",
            "new_string": "async function getData() {\n  try {"
        },
        {
            "old_string": "return data;",
            "new_string": "return data;\n  } catch (error) {\n    console.error('getData error:', error);\n    throw error;\n  }"
        }
    ]
)
```

### Add Logging
```bash
# Add debug logging to function
Edit(
    file_path="/src/utils/process.js",
    old_string="function processData(data) {",
    new_string="function processData(data) {\n  console.log('[processData] Input:', data);"
)
```

### Fix Common ESLint Errors
```python
# Fix unused variables
Edit(
    file_path="/src/components/Example.js",
    old_string="import { unused } from './utils';",
    new_string="// Removed unused import",
    replace_all=True
)

# Add missing semicolons
Bash("npx eslint src/ --fix")
```

## 📝 Debugging Workflow

### 1. Reproduce Issue
```bash
# Check logs
Bash("tail -f logs/app.log | grep ERROR")

# Run specific test
Bash("npm test -- --testNamePattern='failing test'")
```

### 2. Isolate Problem
```bash
# Git bisect
Bash("git bisect start")
Bash("git bisect bad")
Bash("git bisect good HEAD~10")
```

### 3. Add Instrumentation
```python
# Add performance timing
MultiEdit(
    file_path="/src/slow-function.js",
    edits=[
        {
            "old_string": "function slowFunction() {",
            "new_string": "function slowFunction() {\n  console.time('slowFunction');"
        },
        {
            "old_string": "return result;",
            "new_string": "console.timeEnd('slowFunction');\n  return result;"
        }
    ]
)
```

### 4. Test Fix
```bash
# Run tests
Bash("npm test")

# Check specific functionality
Bash("npm run dev")
```

## 🎯 Performance Profiling

### Find Bottlenecks
```bash
# CPU profiling
Bash("node --prof app.js")
Bash("node --prof-process isolate-*.log")

# Memory profiling
Bash("node --expose-gc --trace-gc app.js")
```

### Optimize Queries
```bash
# Find slow queries
Grep("SELECT.*FROM.*JOIN.*JOIN")

# Add query logging
Edit(
    file_path="/src/db/client.js",
    old_string="db.query(sql)",
    new_string="console.time('query'); const result = db.query(sql); console.timeEnd('query'); return result;"
)
```

## 🚨 Emergency Fixes

### Application Won't Start
```bash
# Check for syntax errors
Bash("node -c src/index.js")

# Clear caches
Bash("rm -rf node_modules package-lock.json && npm install")

# Check environment variables
Bash("env | grep -E 'NODE_|PORT|DATABASE'")
```

### Production Issues
```bash
# Roll back deployment
Bash("git revert HEAD")
Bash("git push origin main")

# Emergency patch
Edit(
    file_path="/src/config.js",
    old_string="debug: false",
    new_string="debug: true"
)
```

## 💡 Pro Tips

1. **Always read error messages carefully** - they often contain the solution
2. **Use binary search** to isolate issues (comment out half the code)
3. **Check the basics first** - is the server running? Database connected?
4. **Version control is your friend** - use git bisect to find breaking changes
5. **Don't assume, verify** - add logging to confirm your hypothesis
6. **Read the docs** - especially for third-party libraries
7. **Take breaks** - fresh eyes often spot obvious issues

## 🔗 Useful Bash One-Liners

```bash
# Find recently modified files
find . -type f -mtime -1 -name "*.js"

# Count lines of code
find . -name "*.js" | xargs wc -l | tail -1

# Find large files
find . -type f -size +1M | grep -v node_modules

# Search git history
git log -S "searchterm" --oneline

# Find file by content
find . -name "*.js" -exec grep -l "pattern" {} \;
```