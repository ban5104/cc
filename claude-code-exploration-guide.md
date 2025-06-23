# Claude Code: Advanced Code Exploration, Debugging & Analysis Guide

## Table of Contents
1. [Codebase Navigation Commands](#1-codebase-navigation-commands)
2. [Understanding Large Codebases Quickly](#2-understanding-large-codebases-quickly)
3. [Debugging Strategies](#3-debugging-strategies)
4. [Performance Profiling](#4-performance-profiling)
5. [Code Analysis Tools](#5-code-analysis-tools)
6. [Architecture Exploration](#6-architecture-exploration)
7. [Dependency Analysis](#7-dependency-analysis)
8. [Finding Code Smells](#8-finding-code-smells)
9. [Refactoring Workflows](#9-refactoring-workflows)
10. [Using Claude for Code Reviews](#10-using-claude-for-code-reviews)

---

## 1. Codebase Navigation Commands

### Essential Navigation Tools

#### Using Glob for File Discovery
```bash
# Find all JavaScript/TypeScript files
Glob("**/*.{js,ts,jsx,tsx}")

# Find test files
Glob("**/*.{test,spec}.{js,ts}")

# Find configuration files
Glob("**/{package.json,tsconfig.json,.eslintrc*}")

# Find all Python files in specific directories
Glob("src/**/*.py")
```

#### Using Grep for Content Search
```bash
# Find function definitions
Grep("function\s+\w+|const\s+\w+\s*=\s*\(|class\s+\w+")

# Find imports/exports
Grep("^import|^export", "*.{js,ts}")

# Find TODO comments
Grep("TODO|FIXME|HACK|XXX")

# Find API endpoints
Grep("app\.(get|post|put|delete|patch)\(", "*.js")
```

#### Using LS for Directory Structure
```bash
# List project root with common ignores
LS("/path/to/project", ["node_modules", ".git", "dist", "build"])

# Explore source structure
LS("/path/to/project/src")
```

### Advanced Navigation Patterns

```bash
# Find all files modified in last 24 hours
Bash("find . -type f -mtime -1 -not -path '*/\.*' | head -20")

# Get project structure overview
Bash("find . -type f -name '*.js' -o -name '*.ts' | grep -E '(src/|lib/)' | sort")

# Count lines of code by file type
Bash("find . -name '*.js' -o -name '*.ts' | xargs wc -l | sort -nr")
```

---

## 2. Understanding Large Codebases Quickly

### Initial Exploration Strategy

1. **Start with Entry Points**
```bash
# Find main entry files
Glob("**/index.{js,ts,jsx,tsx}")
Glob("**/main.{js,ts,py}")
Glob("**/app.{js,ts,py}")

# Check package.json for scripts
Read("/path/to/package.json")
```

2. **Understand Project Structure**
```bash
# Get high-level directory structure
LS("/project/root", ["node_modules", ".git"])

# Find key directories
Bash("find . -type d -name 'src' -o -name 'lib' -o -name 'components' | head -10")
```

3. **Identify Core Components**
```bash
# Find main classes/components
Grep("class\s+\w+\s+extends|export\s+default\s+class")

# Find React components
Grep("function\s+\w+\(.*\).*{.*return.*<|const\s+\w+\s*=.*=>.*<")

# Find API routes
Grep("router\.|app\.|@route|@app.route")
```

### Quick Understanding Techniques

1. **Read Configuration First**
```python
# Always start with these files
files_to_read = [
    "package.json",
    "README.md",
    ".env.example",
    "tsconfig.json",
    "webpack.config.js",
    "vite.config.js"
]

for file in files_to_read:
    Read(f"/project/{file}")
```

2. **Follow Import Chains**
```bash
# Find what a module imports
Grep("^import.*from", "src/main.js")

# Find where a module is imported
Grep("from.*['\"].*MyModule['\"]")
```

3. **Understand Data Flow**
```bash
# Find state management
Grep("useState|useReducer|Redux|Vuex|MobX")

# Find API calls
Grep("fetch\(|axios\.|$.ajax|XMLHttpRequest")

# Find database queries
Grep("SELECT|INSERT|UPDATE|DELETE|findOne|findMany|query\(")
```

---

## 3. Debugging Strategies

### Systematic Debugging Approach

1. **Reproduce the Issue**
```bash
# Check error logs
Bash("tail -n 100 logs/error.log | grep -i error")

# Find error handlers
Grep("catch\s*\(|\.catch\(|except\s+\w+:|on\s+error")
```

2. **Trace Execution Path**
```python
# Add debug logging
Edit(
    file_path="/path/to/problematic-file.js",
    old_string="function problemFunction(data) {",
    new_string="function problemFunction(data) {\n  console.log('DEBUG: problemFunction called with:', data);"
)
```

3. **Isolate the Problem**
```bash
# Find recent changes
Bash("git log --since='1 week ago' --oneline")

# Check specific file history
Bash("git log -p path/to/suspicious-file.js | head -50")
```

### Common Debugging Patterns

1. **Type-Related Issues**
```bash
# Find type definitions
Grep("interface\s+\w+|type\s+\w+\s*=")

# Find any type assertions
Grep("as\s+\w+|<\w+>")
```

2. **Async Issues**
```bash
# Find promises without error handling
Grep("\.then\([^)]*\)(?!.*\.catch)")

# Find async functions
Grep("async\s+function|async\s*\(|async\s*\w+\s*=>")
```

3. **State Mutations**
```bash
# Find direct state mutations
Grep("state\.|this\.state\s*=|state\s*=")

# Find array mutations
Grep("\.(push|pop|shift|unshift|splice)\(")
```

### Debug Workflow Example

```python
# 1. Identify error location
error_files = Grep("Error:|Exception:|Failed")

# 2. Read context around error
for file in error_files:
    Read(file, offset=error_line-10, limit=20)

# 3. Add strategic logging
MultiEdit(
    file_path="/path/to/file.js",
    edits=[
        {
            "old_string": "function processData(input) {",
            "new_string": "function processData(input) {\n  console.log('processData input:', input);"
        },
        {
            "old_string": "return result;",
            "new_string": "console.log('processData result:', result);\n  return result;"
        }
    ]
)

# 4. Test with logging
Bash("npm test -- --verbose")
```

---

## 4. Performance Profiling

### Identifying Performance Bottlenecks

1. **Find Expensive Operations**
```bash
# Find loops
Grep("for\s*\(|while\s*\(|\.forEach\(|\.map\(")

# Find recursive functions
Grep("function\s+(\w+).*\{.*\1\(")

# Find synchronous file operations
Grep("readFileSync|writeFileSync|execSync")
```

2. **Database Query Analysis**
```bash
# Find N+1 query patterns
Grep("for.*await.*find|map.*async.*query")

# Find missing indexes
Grep("WHERE|ORDER BY|GROUP BY")
```

3. **Memory Usage Patterns**
```bash
# Find potential memory leaks
Grep("addEventListener|on\(|subscribe\(")

# Find large data structures
Grep("new Array\(|\.push\(|concat\(")
```

### Performance Analysis Workflow

```python
# 1. Add timing measurements
MultiEdit(
    file_path="/path/to/slow-function.js",
    edits=[
        {
            "old_string": "async function slowFunction() {",
            "new_string": "async function slowFunction() {\n  const startTime = performance.now();"
        },
        {
            "old_string": "return result;",
            "new_string": "const endTime = performance.now();\n  console.log(`slowFunction took ${endTime - startTime}ms`);\n  return result;"
        }
    ]
)

# 2. Profile specific operations
performance_code = """
console.time('dataProcessing');
// existing code
console.timeEnd('dataProcessing');
"""
```

---

## 5. Code Analysis Tools

### Static Analysis Integration

1. **ESLint Integration**
```bash
# Run ESLint
Bash("npx eslint src/ --ext .js,.jsx,.ts,.tsx")

# Find ESLint disable comments
Grep("eslint-disable")

# Check for specific rules
Bash("npx eslint src/ --rule 'no-unused-vars: error'")
```

2. **Type Checking**
```bash
# Run TypeScript compiler
Bash("npx tsc --noEmit")

# Find type errors
Bash("npx tsc --noEmit | grep error")
```

3. **Security Analysis**
```bash
# Find potential security issues
Grep("eval\(|innerHTML\s*=|dangerouslySetInnerHTML")

# Check for hardcoded secrets
Grep("password\s*=\s*['\"]|api_key\s*=\s*['\"]|secret\s*=\s*['\"]")
```

### Custom Analysis Scripts

```python
# Create analysis script
Write(
    file_path="/tmp/analyze_complexity.py",
    content="""
import os
import re

def count_functions(content):
    return len(re.findall(r'function\s+\w+|const\s+\w+\s*=\s*\(', content))

def analyze_file(filepath):
    with open(filepath, 'r') as f:
        content = f.read()
    
    lines = len(content.splitlines())
    functions = count_functions(content)
    complexity = lines / max(functions, 1)
    
    return {
        'file': filepath,
        'lines': lines,
        'functions': functions,
        'avg_function_length': complexity
    }

# Usage example
for root, dirs, files in os.walk('src'):
    for file in files:
        if file.endswith('.js'):
            stats = analyze_file(os.path.join(root, file))
            if stats['avg_function_length'] > 50:
                print(f"Complex file: {stats['file']} ({stats['avg_function_length']:.1f} lines/function)")
"""
)
```

---

## 6. Architecture Exploration

### Understanding System Architecture

1. **Identify Architectural Patterns**
```bash
# MVC pattern
Grep("controller|Controller|model|Model|view|View")

# Microservices
Grep("service|Service|api|API|endpoint")

# Event-driven
Grep("emit|on\(|addEventListener|subscribe|publish")
```

2. **Map Component Dependencies**
```python
# Create dependency graph
def analyze_imports(file_path):
    content = Read(file_path)
    imports = Grep("^import.*from", file_path)
    return {
        'file': file_path,
        'imports': imports,
        'exported': Grep("^export", file_path)
    }
```

3. **Identify Layers**
```bash
# Find data layer
Glob("**/models/**/*.{js,ts}")
Glob("**/repositories/**/*.{js,ts}")

# Find business logic
Glob("**/services/**/*.{js,ts}")
Glob("**/controllers/**/*.{js,ts}")

# Find presentation layer
Glob("**/views/**/*.{js,jsx,tsx}")
Glob("**/components/**/*.{js,jsx,tsx}")
```

### Architecture Documentation

```python
# Generate architecture overview
Write(
    file_path="/tmp/architecture_overview.md",
    content="""# Architecture Overview

## Layer Structure
- **Presentation**: React components in /src/components
- **Business Logic**: Services in /src/services
- **Data Access**: Repositories in /src/repositories
- **Models**: Data models in /src/models

## Key Components
1. Authentication: /src/auth
2. API Gateway: /src/api
3. State Management: /src/store
4. Utilities: /src/utils
"""
)
```

---

## 7. Dependency Analysis

### External Dependencies

1. **Analyze package.json**
```python
import json

# Read and analyze dependencies
package_json = json.loads(Read("/path/to/package.json"))
deps = package_json.get('dependencies', {})
dev_deps = package_json.get('devDependencies', {})

# Find unused dependencies
all_imports = Grep("from ['\"]([^'\"]+)['\"]|require\(['\"]([^'\"]+)['\"]\)")
```

2. **Check for Vulnerabilities**
```bash
# Run audit
Bash("npm audit")

# Check specific package
Bash("npm ls package-name")
```

3. **Dependency Tree**
```bash
# Full dependency tree
Bash("npm ls --depth=2")

# Find duplicate packages
Bash("npm ls --depth=999 | grep -E '@[0-9]+\.[0-9]+\.[0-9]+' | sort | uniq -d")
```

### Internal Dependencies

```python
# Map internal module dependencies
def create_dependency_map():
    files = Glob("src/**/*.{js,ts}")
    dep_map = {}
    
    for file in files:
        imports = Grep("from ['\"]\.\.?/", file)
        dep_map[file] = imports
    
    return dep_map
```

---

## 8. Finding Code Smells

### Common Code Smells

1. **Long Functions**
```python
# Find functions over 50 lines
def find_long_functions(file_path):
    content = Read(file_path)
    lines = content.split('\n')
    
    function_starts = []
    for i, line in enumerate(lines):
        if re.match(r'(function|const.*=.*=>|class)', line):
            function_starts.append(i)
    
    for i in range(len(function_starts) - 1):
        length = function_starts[i+1] - function_starts[i]
        if length > 50:
            print(f"Long function at {file_path}:{function_starts[i]} ({length} lines)")
```

2. **Duplicate Code**
```bash
# Find similar code patterns
Grep("copy.*paste|TODO.*refactor|FIXME.*duplicate")

# Find repeated patterns
Bash("find . -name '*.js' -exec grep -l 'pattern' {} \; | xargs grep -n 'pattern'")
```

3. **Complex Conditionals**
```bash
# Find nested conditionals
Grep("if.*if.*if|&&.*&&.*&&|\|\|.*\|\|.*\|\|")

# Find complex ternary operators
Grep("\?.*\?.*\:")
```

### Code Smell Detection Patterns

```python
# Comprehensive code smell detector
code_smells = {
    "long_lines": r".{120,}",
    "magic_numbers": r"[^0-9][0-9]{2,}[^0-9]",
    "todo_comments": r"TODO|FIXME|HACK|XXX",
    "console_logs": r"console\.(log|error|warn)",
    "commented_code": r"^\s*//.*[{};]",
    "empty_catch": r"catch\s*\([^)]*\)\s*{\s*}",
    "var_usage": r"\bvar\s+",
    "eval_usage": r"\beval\s*\(",
    "hardcoded_values": r"(password|secret|key)\s*=\s*['\"][^'\"]+['\"]"
}

for smell_name, pattern in code_smells.items():
    results = Grep(pattern)
    if results:
        print(f"\nCode smell '{smell_name}' found in:")
        for file in results:
            print(f"  - {file}")
```

---

## 9. Refactoring Workflows

### Safe Refactoring Process

1. **Pre-refactoring Checklist**
```bash
# Ensure tests pass
Bash("npm test")

# Create backup branch
Bash("git checkout -b refactor/backup-$(date +%Y%m%d)")

# Check current coverage
Bash("npm run coverage")
```

2. **Extract Function Refactoring**
```python
# Example: Extract repeated code into function
MultiEdit(
    file_path="/path/to/file.js",
    edits=[
        {
            "old_string": "// Repeated code block 1\nconst result1 = data.map(x => x * 2).filter(x => x > 10);",
            "new_string": "const result1 = processData(data);"
        },
        {
            "old_string": "// Repeated code block 2\nconst result2 = items.map(x => x * 2).filter(x => x > 10);",
            "new_string": "const result2 = processData(items);"
        },
        {
            "old_string": "function existingFunction() {",
            "new_string": "function processData(arr) {\n  return arr.map(x => x * 2).filter(x => x > 10);\n}\n\nfunction existingFunction() {"
        }
    ]
)
```

3. **Rename Variable Refactoring**
```python
# Safe variable renaming
Edit(
    file_path="/path/to/file.js",
    old_string="temp",
    new_string="processedUserData",
    replace_all=True
)
```

### Refactoring Patterns

1. **Replace Magic Numbers**
```python
MultiEdit(
    file_path="/path/to/file.js",
    edits=[
        {
            "old_string": "if (count > 100) {",
            "new_string": "const MAX_ITEM_COUNT = 100;\n\nif (count > MAX_ITEM_COUNT) {"
        },
        {
            "old_string": "return price * 0.15;",
            "new_string": "const TAX_RATE = 0.15;\nreturn price * TAX_RATE;"
        }
    ]
)
```

2. **Extract Component (React)**
```python
# Extract reusable component
Write(
    file_path="/src/components/UserAvatar.jsx",
    content="""import React from 'react';

const UserAvatar = ({ user, size = 'medium' }) => {
  const sizeClasses = {
    small: 'w-8 h-8',
    medium: 'w-12 h-12',
    large: 'w-16 h-16'
  };

  return (
    <img
      src={user.avatar || '/default-avatar.png'}
      alt={user.name}
      className={`rounded-full ${sizeClasses[size]}`}
    />
  );
};

export default UserAvatar;
"""
)

# Update original file
Edit(
    file_path="/src/components/UserList.jsx",
    old_string='<img src={user.avatar} alt={user.name} className="rounded-full w-12 h-12" />',
    new_string='<UserAvatar user={user} size="medium" />',
    replace_all=True
)
```

---

## 10. Using Claude for Code Reviews

### Automated Code Review Process

1. **Review Checklist**
```python
review_checklist = {
    "Style": [
        "Consistent naming conventions",
        "Proper indentation",
        "No commented-out code",
        "Meaningful variable names"
    ],
    "Logic": [
        "No obvious bugs",
        "Edge cases handled",
        "Error handling present",
        "No infinite loops"
    ],
    "Performance": [
        "No unnecessary loops",
        "Efficient algorithms",
        "Proper async handling",
        "No memory leaks"
    ],
    "Security": [
        "Input validation",
        "No hardcoded secrets",
        "Proper authentication",
        "SQL injection prevention"
    ],
    "Maintainability": [
        "Clear documentation",
        "Modular design",
        "DRY principle",
        "SOLID principles"
    ]
}
```

2. **Automated Review Script**
```python
def review_file(file_path):
    content = Read(file_path)
    issues = []
    
    # Check for long functions
    if "function" in content:
        functions = content.split("function")
        for func in functions[1:]:
            lines = func.split("\n")
            if len(lines) > 50:
                issues.append("Long function detected (>50 lines)")
    
    # Check for console.log
    if "console.log" in content:
        issues.append("Console.log statements found")
    
    # Check for TODO comments
    if "TODO" in content or "FIXME" in content:
        issues.append("Unresolved TODO/FIXME comments")
    
    return issues
```

3. **Review Workflow**
```bash
# Get changed files
changed_files=$(git diff --name-only HEAD~1)

# Review each file
for file in $changed_files; do
    echo "Reviewing $file..."
    
    # Check file size
    lines=$(wc -l < "$file")
    if [ $lines -gt 500 ]; then
        echo "  ⚠️  Large file: $lines lines"
    fi
    
    # Run linter
    npx eslint "$file"
    
    # Check complexity
    npx complexity-report "$file"
done
```

### Code Review Examples

1. **Security Review**
```python
# Check for security issues
security_patterns = [
    ("eval(", "Avoid using eval() - security risk"),
    ("innerHTML =", "Use textContent instead of innerHTML"),
    ("http://", "Use HTTPS instead of HTTP"),
    ("password.*=.*['\"]", "Don't hardcode passwords"),
    ("SELECT.*\\*.*FROM", "Avoid SELECT * queries")
]

for pattern, message in security_patterns:
    results = Grep(pattern)
    if results:
        print(f"\n⚠️  Security Issue: {message}")
        for file in results:
            print(f"   Found in: {file}")
```

2. **Performance Review**
```python
# Check for performance issues
performance_patterns = [
    ("for.*for", "Nested loops detected"),
    ("await.*map", "Avoid await in loops"),
    ("document.querySelector.*for", "DOM queries in loops"),
    ("new Date\\(\\).*for", "Date creation in loops")
]

for pattern, message in performance_patterns:
    results = Grep(pattern)
    if results:
        print(f"\n⚠️  Performance Issue: {message}")
```

### Best Practices for Code Reviews

1. **Review Pull Requests**
```bash
# Get PR diff
Bash("gh pr diff 123")

# Check PR files
Bash("gh pr view 123 --json files")

# Add review comments
Bash("gh pr review 123 --comment -b 'Looks good, but consider...'")
```

2. **Automated Review Comments**
```python
def generate_review_comment(issue_type, file, line, description):
    return f"""
## {issue_type} Issue Found

**File**: `{file}`
**Line**: {line}

**Description**: {description}

**Suggestion**: [Provide specific fix]

**Reference**: [Link to best practices]
"""
```

## Summary

This guide provides comprehensive techniques for exploring, debugging, and analyzing codebases with Claude Code. Key takeaways:

1. **Navigation**: Use Glob, Grep, and LS strategically to understand codebase structure
2. **Understanding**: Start with entry points and follow the dependency chain
3. **Debugging**: Systematic approach with logging, isolation, and reproduction
4. **Performance**: Profile bottlenecks and optimize critical paths
5. **Analysis**: Leverage static analysis tools and custom scripts
6. **Architecture**: Map components and understand system design
7. **Dependencies**: Analyze both external and internal dependencies
8. **Code Smells**: Detect and document technical debt
9. **Refactoring**: Follow safe, incremental refactoring processes
10. **Reviews**: Automate review processes and maintain quality standards

Remember to always:
- Test before and after changes
- Use version control effectively
- Document your findings
- Communicate with your team
- Focus on incremental improvements