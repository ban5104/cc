# Practical Code Analysis Examples with Claude Code

## Real-World Scenarios and Solutions

### Scenario 1: Exploring a New React Project

```bash
# Step 1: Get project overview
Read("package.json")  # Understand dependencies and scripts
LS("/project/src", ["__tests__", "*.test.js"])  # See structure

# Step 2: Find main components
Glob("src/components/**/*.{jsx,tsx}")
Glob("src/pages/**/*.{jsx,tsx}")

# Step 3: Understand routing
Grep("Route|Router|createBrowserRouter", "src/**/*.{js,jsx,ts,tsx}")

# Step 4: Find state management
Grep("useState|useReducer|useContext|Redux|Zustand", "src/**/*.{js,jsx}")

# Step 5: Locate API integration
Grep("fetch|axios|useSWR|useQuery", "src/**/*.{js,jsx}")
```

### Scenario 2: Debugging a Production Issue

```bash
# Step 1: Check error logs
Bash("tail -n 500 /var/log/app/error.log | grep -i 'error\\|exception' | tail -20")

# Step 2: Find error handlers
Grep("catch|.catch\\(|try\\s*{", "src/**/*.js")

# Step 3: Check recent deployments
Bash("git log --since='2 days ago' --oneline")

# Step 4: Find the problematic code
error_message = "Cannot read property 'user' of undefined"
Grep("\.user[^a-zA-Z]", "src/**/*.js")

# Step 5: Add defensive checks
MultiEdit(
    file_path="/src/components/UserProfile.js",
    edits=[
        {
            "old_string": "const username = data.user.name;",
            "new_string": "const username = data?.user?.name || 'Anonymous';"
        },
        {
            "old_string": "if (response.user.id === currentUser.id) {",
            "new_string": "if (response?.user?.id && currentUser?.id && response.user.id === currentUser.id) {"
        }
    ]
)
```

### Scenario 3: Performance Optimization

```python
# Step 1: Find slow database queries
slow_queries = Grep("SELECT.*JOIN.*JOIN|SELECT.*FROM.*WHERE.*IN\\s*\\(", "src/**/*.{js,ts}")

# Step 2: Identify N+1 queries
n_plus_one = Grep("for.*await.*findOne|map.*async.*query", "src/**/*.{js,ts}")

# Step 3: Find unnecessary re-renders (React)
rerenders = Grep("useState.*useEffect|useEffect.*\\[\\]", "src/**/*.{jsx,tsx}")

# Step 4: Add memoization
MultiEdit(
    file_path="/src/components/ExpensiveComponent.jsx",
    edits=[
        {
            "old_string": "import React from 'react';",
            "new_string": "import React, { useMemo, useCallback } from 'react';"
        },
        {
            "old_string": "const processedData = data.map(item => expensiveOperation(item));",
            "new_string": "const processedData = useMemo(() => data.map(item => expensiveOperation(item)), [data]);"
        },
        {
            "old_string": "const handleClick = () => {",
            "new_string": "const handleClick = useCallback(() => {"
        }
    ]
)
```

### Scenario 4: Security Audit

```python
# Step 1: Find authentication bypasses
auth_issues = Grep("// TODO.*auth|// FIXME.*security|skipAuth|bypass", "src/**/*.{js,ts}")

# Step 2: Check for SQL injection vulnerabilities
sql_injection = Grep("query\\(`.*\\${|query\\(\".*\" \\+", "src/**/*.{js,ts}")

# Step 3: Find XSS vulnerabilities
xss_vulnerabilities = Grep("innerHTML|dangerouslySetInnerHTML|eval\\(|new Function\\(", "src/**/*.{js,jsx,ts,tsx}")

# Step 4: Check for exposed secrets
secrets = Grep("api_key.*=.*['\"]|password.*=.*['\"]|secret.*=.*['\"]", "**/*.{js,env,json}")

# Step 5: Fix SQL injection
Edit(
    file_path="/src/db/queries.js",
    old_string='db.query(`SELECT * FROM users WHERE id = ${userId}`)',
    new_string='db.query("SELECT * FROM users WHERE id = ?", [userId])',
    replace_all=True
)
```

### Scenario 5: Refactoring Legacy Code

```python
# Step 1: Identify code to refactor
legacy_patterns = {
    "callbacks": Grep("function.*callback\\)|\\(err, data\\)", "src/**/*.js"),
    "var_usage": Grep("\\bvar\\s+", "src/**/*.js"),
    "prototype": Grep("\\.prototype\\.", "src/**/*.js"),
    "jquery": Grep("\\$\\(|jQuery\\(", "src/**/*.js")
}

# Step 2: Convert callbacks to promises
MultiEdit(
    file_path="/src/utils/fileOperations.js",
    edits=[
        {
            "old_string": "function readFile(path, callback) {\n  fs.readFile(path, 'utf8', (err, data) => {\n    if (err) return callback(err);\n    callback(null, data);\n  });",
            "new_string": "function readFile(path) {\n  return new Promise((resolve, reject) => {\n    fs.readFile(path, 'utf8', (err, data) => {\n      if (err) reject(err);\n      else resolve(data);\n    });\n  });"
        }
    ]
)

# Step 3: Convert to ES6 modules
MultiEdit(
    file_path="/src/utils/helpers.js",
    edits=[
        {
            "old_string": "var utils = require('./utils');",
            "new_string": "import utils from './utils';"
        },
        {
            "old_string": "module.exports = {",
            "new_string": "export {"
        }
    ]
)
```

### Scenario 6: Code Review Automation

```python
# Create automated review script
Write(
    file_path="/tmp/auto_review.py",
    content='''#!/usr/bin/env python3
import subprocess
import json
import re

class CodeReviewer:
    def __init__(self):
        self.issues = []
    
    def check_file_size(self, file_path):
        """Check if file is too large"""
        with open(file_path, 'r') as f:
            lines = len(f.readlines())
        if lines > 500:
            self.issues.append({
                'file': file_path,
                'type': 'warning',
                'message': f'File too large: {lines} lines'
            })
    
    def check_function_length(self, file_path):
        """Check for long functions"""
        with open(file_path, 'r') as f:
            content = f.read()
        
        # Simple function detection
        functions = re.findall(r'function\s+(\w+).*?{(.*?)}', content, re.DOTALL)
        for func_name, func_body in functions:
            lines = len(func_body.strip().split('\\n'))
            if lines > 50:
                self.issues.append({
                    'file': file_path,
                    'type': 'warning',
                    'message': f'Function {func_name} is too long: {lines} lines'
                })
    
    def check_console_logs(self, file_path):
        """Check for console.log statements"""
        with open(file_path, 'r') as f:
            content = f.read()
        
        console_logs = re.findall(r'console\.(log|error|warn)', content)
        if console_logs:
            self.issues.append({
                'file': file_path,
                'type': 'error',
                'message': f'Found {len(console_logs)} console statements'
            })
    
    def review_file(self, file_path):
        """Run all checks on a file"""
        if file_path.endswith(('.js', '.jsx', '.ts', '.tsx')):
            self.check_file_size(file_path)
            self.check_function_length(file_path)
            self.check_console_logs(file_path)
    
    def generate_report(self):
        """Generate review report"""
        if not self.issues:
            return "✅ No issues found!"
        
        report = "## Code Review Report\\n\\n"
        for issue in self.issues:
            emoji = "⚠️" if issue['type'] == 'warning' else "❌"
            report += f"{emoji} **{issue['file']}**: {issue['message']}\\n"
        
        return report

# Usage
reviewer = CodeReviewer()
# Add files to review
reviewer.review_file('/path/to/file.js')
print(reviewer.generate_report())
'''
)
```

### Scenario 7: Architecture Analysis

```python
# Create architecture analyzer
Write(
    file_path="/tmp/architecture_analyzer.py",
    content='''#!/usr/bin/env python3
import os
import json
from collections import defaultdict

class ArchitectureAnalyzer:
    def __init__(self, root_path):
        self.root_path = root_path
        self.layers = defaultdict(list)
        self.dependencies = defaultdict(set)
    
    def analyze_structure(self):
        """Analyze directory structure to identify layers"""
        for root, dirs, files in os.walk(self.root_path):
            rel_path = os.path.relpath(root, self.root_path)
            
            # Identify architectural layers
            if 'controller' in rel_path.lower():
                self.layers['controllers'].extend(files)
            elif 'service' in rel_path.lower():
                self.layers['services'].extend(files)
            elif 'model' in rel_path.lower():
                self.layers['models'].extend(files)
            elif 'view' in rel_path.lower() or 'component' in rel_path.lower():
                self.layers['views'].extend(files)
            elif 'util' in rel_path.lower() or 'helper' in rel_path.lower():
                self.layers['utilities'].extend(files)
    
    def analyze_dependencies(self, file_path):
        """Extract import dependencies"""
        with open(file_path, 'r') as f:
            content = f.read()
        
        # Find imports
        imports = re.findall(r"import.*from ['\"]([^'\"]+)['\"]", content)
        requires = re.findall(r"require\\(['\"]([^'\"]+)['\"]\\)", content)
        
        return imports + requires
    
    def generate_report(self):
        """Generate architecture report"""
        report = {
            'layers': dict(self.layers),
            'statistics': {
                'total_files': sum(len(files) for files in self.layers.values()),
                'layer_distribution': {
                    layer: len(files) for layer, files in self.layers.items()
                }
            }
        }
        return json.dumps(report, indent=2)

# Usage
analyzer = ArchitectureAnalyzer('/project/src')
analyzer.analyze_structure()
print(analyzer.generate_report())
'''
)
```

### Scenario 8: Dependency Visualization

```python
# Create dependency graph generator
Write(
    file_path="/tmp/dependency_graph.py",
    content='''#!/usr/bin/env python3
import os
import re
from collections import defaultdict

def generate_mermaid_graph(dependencies):
    """Generate Mermaid diagram for dependencies"""
    graph = ["graph TD"]
    
    for source, deps in dependencies.items():
        source_name = os.path.basename(source).replace('.', '_')
        for dep in deps:
            dep_name = os.path.basename(dep).replace('.', '_')
            graph.append(f"    {source_name} --> {dep_name}")
    
    return "\\n".join(graph)

def analyze_imports(root_dir):
    """Analyze all imports in the project"""
    dependencies = defaultdict(set)
    
    for root, dirs, files in os.walk(root_dir):
        for file in files:
            if file.endswith(('.js', '.jsx', '.ts', '.tsx')):
                file_path = os.path.join(root, file)
                
                with open(file_path, 'r') as f:
                    content = f.read()
                
                # Find relative imports
                imports = re.findall(r"from ['\"](\\.\\./[^'\"]+)['\"]", content)
                for imp in imports:
                    dependencies[file].add(imp)
    
    return dependencies

# Generate graph
deps = analyze_imports('/project/src')
mermaid_graph = generate_mermaid_graph(deps)
print(mermaid_graph)
'''
)
```

## Quick Reference Commands

### Most Useful Claude Code Commands for Analysis

```bash
# Find all TODOs and FIXMEs
Grep("TODO|FIXME|HACK|XXX|BUG|DEPRECATED")

# Find all test files
Glob("**/*.{test,spec}.{js,ts,jsx,tsx}")

# Find configuration files
Glob("**/{.*rc.json,*.config.js,*.conf.js}")

# Find environment variables usage
Grep("process\\.env\\.|import\\.meta\\.env\\.")

# Find all API endpoints
Grep("(app|router)\\.(get|post|put|delete|patch)\\(")

# Find all database models
Glob("**/models/**/*.{js,ts}")

# Find error boundaries (React)
Grep("componentDidCatch|ErrorBoundary")

# Find memory leaks
Grep("addEventListener(?!.*removeEventListener)")

# Find circular dependencies
Bash("npx madge --circular src/")

# Find unused exports
Bash("npx ts-prune")
```

## Tips for Effective Code Analysis

1. **Start Broad, Then Narrow**
   - Begin with project structure
   - Identify key components
   - Deep dive into specific areas

2. **Follow the Data Flow**
   - Start from user input
   - Trace through processing
   - End at output/storage

3. **Use Multiple Tools Together**
   - Combine Glob for files
   - Grep for content
   - Bash for advanced analysis

4. **Document Your Findings**
   - Create analysis notes
   - Generate diagrams
   - Share with team

5. **Automate Repetitive Tasks**
   - Create analysis scripts
   - Set up pre-commit hooks
   - Use CI/CD for continuous analysis