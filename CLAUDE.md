# CLAUDE.md - AI Assistant Guide

This document provides comprehensive information about the repository structure, development workflows, and conventions for AI assistants working with this codebase.

## Repository Overview

**Project Name:** my-project_1
**Current State:** Empty repository ready for development
**Last Updated:** 2025-11-13

## Current Repository State

The repository is a blank slate ready for new development.

## Directory Structure

```
my-project_1/
├── .git/                 # Git repository data
└── CLAUDE.md            # This file (AI assistant guide)
```

### Expected Future Structure

When development begins, consider organizing the project as follows:

```
my-project_1/
├── .git/
├── .github/             # GitHub Actions, issue templates, PR templates
│   └── workflows/       # CI/CD workflows
├── docs/                # Documentation
├── src/                 # Source code
│   ├── components/      # UI components (if applicable)
│   ├── services/        # Business logic/services
│   ├── utils/           # Utility functions
│   └── config/          # Configuration files
├── tests/               # Test files
│   ├── unit/           # Unit tests
│   ├── integration/    # Integration tests
│   └── e2e/            # End-to-end tests
├── scripts/             # Build and deployment scripts
├── .gitignore          # Git ignore rules
├── README.md           # Project documentation
├── CLAUDE.md           # This file
├── CONTRIBUTING.md     # Contribution guidelines
└── package.json        # Dependencies (if Node.js project)
```

## Development Workflows

### Branch Strategy

**Current Branch:** `claude/claude-md-mhxzpfxjb00h9522-01VEvGjbHmxo7eEM4WryxFYF`

#### Branch Naming Conventions
- `main` - Production-ready code
- `develop` - Integration branch for features
- `feature/*` - New features
- `bugfix/*` - Bug fixes
- `hotfix/*` - Urgent production fixes
- `claude/*` - AI assistant development branches (follow format: `claude/claude-md-{session-id}`)

#### AI Assistant Branch Rules
1. **ALWAYS** work on the designated `claude/*` branch
2. **NEVER** push directly to `main` or `develop` without explicit permission
3. Branch names must start with `claude/` and end with the matching session ID
4. Create the branch locally if it doesn't exist: `git checkout -b <branch-name>`

### Git Operations Best Practices

#### Pushing Changes
```bash
# Always use the -u flag for new branches
git push -u origin <branch-name>

# Branch name MUST start with 'claude/' and end with session ID
# Otherwise push will fail with 403 error
```

#### Network Retry Logic
- For `git push`, `git fetch`, or `git pull` failures, retry up to 4 times
- Use exponential backoff: 2s, 4s, 8s, 16s
- Only retry on network errors, not authentication or permission errors

#### Fetching Updates
```bash
# Prefer fetching specific branches
git fetch origin <branch-name>

# For pulling updates
git pull origin <branch-name>
```

### Commit Message Conventions

Follow these conventions for clear, consistent commit history:

#### Format
```
<type>(<scope>): <subject>

<body>

<footer>
```

#### Types
- `feat` - New feature
- `fix` - Bug fix
- `docs` - Documentation changes
- `style` - Code style changes (formatting, missing semicolons, etc.)
- `refactor` - Code refactoring without changing functionality
- `perf` - Performance improvements
- `test` - Adding or updating tests
- `chore` - Maintenance tasks, dependency updates
- `ci` - CI/CD changes

#### Examples
```bash
feat(api): add user authentication endpoint

fix(ui): resolve button alignment issue on mobile

docs(readme): update installation instructions

refactor(services): simplify data processing logic
```

### Pull Request Workflow

When creating pull requests:

1. **Branch Preparation**
   ```bash
   git status                    # Check current state
   git diff                      # Review changes
   git log --oneline -10        # Review commits
   git diff main...HEAD         # Compare with base branch
   ```

2. **Create PR**
   ```bash
   gh pr create --title "Brief description" --body "$(cat <<'EOF'
   ## Summary
   - Key change 1
   - Key change 2

   ## Test Plan
   - [ ] Test case 1
   - [ ] Test case 2

   ## Related Issues
   Closes #123
   EOF
   )"
   ```

3. **PR Title Format**
   - Use the same conventions as commit messages
   - Be descriptive but concise
   - Example: `feat(auth): implement OAuth2 authentication`

4. **PR Description Template**
   ```markdown
   ## Summary
   Brief description of changes (2-3 bullet points)

   ## Changes Made
   - Detailed change 1
   - Detailed change 2

   ## Test Plan
   - [ ] Unit tests pass
   - [ ] Integration tests pass
   - [ ] Manual testing completed

   ## Screenshots (if UI changes)
   [Add screenshots here]

   ## Related Issues
   Closes #123, Relates to #456

   ## Breaking Changes
   [List any breaking changes or write "None"]

   ## Additional Notes
   [Any other relevant information]
   ```

## Code Conventions

### General Principles

1. **Code Quality**
   - Write clean, readable, and maintainable code
   - Follow DRY (Don't Repeat Yourself) principle
   - Keep functions small and focused
   - Use meaningful variable and function names

2. **Security**
   - NEVER introduce security vulnerabilities
   - Watch for: SQL injection, XSS, CSRF, command injection
   - Follow OWASP Top 10 guidelines
   - Validate and sanitize all inputs
   - Use parameterized queries
   - Implement proper authentication and authorization

3. **Error Handling**
   - Handle errors gracefully
   - Provide meaningful error messages
   - Log errors appropriately
   - Don't expose sensitive information in errors

4. **Comments and Documentation**
   - Write self-documenting code
   - Add comments for complex logic
   - Document public APIs
   - Keep comments up-to-date with code changes

### Language-Specific Conventions

#### JavaScript/TypeScript
```javascript
// Use const by default, let when reassignment needed
const API_URL = 'https://api.example.com';
let counter = 0;

// Use arrow functions for callbacks
array.map(item => item.value);

// Prefer async/await over promise chains
async function fetchData() {
  try {
    const response = await fetch(API_URL);
    const data = await response.json();
    return data;
  } catch (error) {
    console.error('Failed to fetch data:', error);
    throw error;
  }
}

// Use template literals
const message = `Hello, ${name}!`;

// Destructure when appropriate
const { id, name, email } = user;
```

#### Python
```python
# Follow PEP 8 style guide
# Use snake_case for functions and variables
def calculate_total(items):
    return sum(item.price for item in items)

# Use list comprehensions when readable
squares = [x**2 for x in range(10)]

# Type hints for better documentation
def process_data(data: dict[str, Any]) -> list[str]:
    return list(data.keys())

# Context managers for resource management
with open('file.txt', 'r') as f:
    content = f.read()
```

## Testing Guidelines

### Test Structure
```
tests/
├── unit/           # Fast, isolated tests
├── integration/    # Tests for component interaction
└── e2e/           # Full system tests
```

### Testing Principles
1. **Write tests first** (TDD when appropriate)
2. **Test behavior, not implementation**
3. **Keep tests independent** and idempotent
4. **Use descriptive test names**
5. **Follow AAA pattern:** Arrange, Act, Assert
6. **Mock external dependencies**

### Test Naming
```javascript
// Good test names describe what they test
describe('UserService', () => {
  describe('createUser', () => {
    it('should create user with valid data', () => {});
    it('should throw error when email is invalid', () => {});
    it('should hash password before saving', () => {});
  });
});
```

## AI Assistant Specific Guidelines

### Task Management

1. **Always use TodoWrite** for multi-step tasks
   - Break down complex tasks into smaller steps
   - Mark tasks as in_progress before starting
   - Mark tasks as completed immediately after finishing
   - Only ONE task should be in_progress at a time

2. **Task Completion Rules**
   - ONLY mark completed when fully accomplished
   - If blocked or errors occur, keep as in_progress
   - Create new tasks for discovered work

### Tool Usage Priority

1. **File Operations**
   - Use `Read` instead of `cat`, `head`, or `tail`
   - Use `Edit` instead of `sed` or `awk`
   - Use `Write` instead of `echo >` or `cat <<EOF`
   - Use `Glob` instead of `find` or `ls` for file patterns
   - Use `Grep` instead of bash `grep` or `rg`

2. **Search Operations**
   - Use `Task` tool with `subagent_type=Explore` for open-ended codebase exploration
   - Use `Glob` for file pattern matching
   - Use `Grep` for content search
   - Run multiple searches in parallel when independent

3. **Communication**
   - Output text directly, never use `echo` to communicate
   - Use markdown for formatting
   - Be concise and clear
   - Reference code locations as `file_path:line_number`

### Code Reference Format

When discussing code, use this format:
```
The authentication logic is in src/auth/login.ts:45
```

### File Creation Guidelines

1. **AVOID creating files unless absolutely necessary**
2. **PREFER editing existing files** over creating new ones
3. **Never proactively create documentation** unless explicitly requested
4. **No emoji** unless user explicitly requests them

### Parallel Operations

When multiple independent operations are needed:
```python
# Good: Multiple independent tool calls in one message
Read(file1)
Read(file2)
Read(file3)

# Bad: Sequential calls when they could be parallel
Read(file1)
# wait
Read(file2)
# wait
Read(file3)
```

### Security Context

This codebase is for:
- ✅ Authorized security testing
- ✅ Defensive security
- ✅ CTF challenges
- ✅ Educational contexts
- ❌ Destructive techniques
- ❌ DoS attacks
- ❌ Mass targeting
- ❌ Supply chain compromise
- ❌ Detection evasion for malicious purposes

## Environment Information

**Platform:** Linux 4.4.0
**Working Directory:** `/home/user/my-project_1`
**Git Repository:** Yes
**AI Model:** Claude Sonnet 4.5 (claude-sonnet-4-5-20250929)

## Common Commands Reference

### Git Commands
```bash
# Check status
git status

# View changes
git diff
git diff --staged

# Stage changes
git add <file>
git add .

# Commit
git commit -m "type(scope): message"

# Push with upstream
git push -u origin <branch-name>

# View history
git log --oneline --graph
git log -10

# Create branch
git checkout -b <branch-name>

# Switch branch
git checkout <branch-name>

# View remote
git remote -v
```

### File Operations
```bash
# List files
ls -la

# Find files by pattern
find . -name "*.js"

# Search in files
grep -r "pattern" .

# View file
cat <file>
head -n 20 <file>
tail -n 20 <file>
```

## Development Checklist

When starting work on this repository:

- [ ] Verify you're on the correct `claude/*` branch
- [ ] Understand the task requirements
- [ ] Create a task list with TodoWrite for complex tasks
- [ ] Read existing code before making changes
- [ ] Follow code conventions and style guides
- [ ] Check for security vulnerabilities
- [ ] Write or update tests
- [ ] Run tests to ensure they pass
- [ ] Review your changes with `git diff`
- [ ] Commit with clear, conventional commit messages
- [ ] Push to the correct branch
- [ ] Create a detailed pull request (if needed)

## Project Setup (To Be Defined)

As this is currently an empty repository, document the following when the project structure is established:

### Technology Stack
- [ ] Language(s): _________________
- [ ] Framework(s): _________________
- [ ] Database: _________________
- [ ] Testing Framework: _________________
- [ ] Build Tool: _________________
- [ ] Package Manager: _________________

### Installation Steps
```bash
# To be added when project structure is defined
```

### Running the Project
```bash
# Development server
# To be added

# Build
# To be added

# Test
# To be added
```

### Environment Variables
```bash
# List required environment variables here
```

## Troubleshooting

### Common Issues

#### Git Push Fails with 403
- **Cause:** Branch name doesn't start with `claude/` or doesn't match session ID
- **Solution:** Ensure branch name follows format: `claude/claude-md-{session-id}`

#### Network Errors on Git Operations
- **Solution:** Retry with exponential backoff (2s, 4s, 8s, 16s)
- **Max Retries:** 4 attempts

#### Pre-commit Hook Failures
- **Solution:** Review the hook output, fix issues, and retry
- **Note:** Never use `--no-verify` unless explicitly requested

## Resources

### Documentation
- Repository: http://local_proxy@127.0.0.1:37656/git/CyberGunz7/my-project_1
- Claude Code Docs: https://docs.claude.com/en/docs/claude-code/

### Useful Links
- Git Conventions: https://www.conventionalcommits.org/
- OWASP Top 10: https://owasp.org/www-project-top-ten/
- Code Review Best Practices: _[To be added]_

## Contributing

### For Human Developers
1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Write tests
5. Submit a pull request

### For AI Assistants
1. Work on assigned `claude/*` branch
2. Use TodoWrite for task tracking
3. Follow all conventions in this document
4. Create detailed commits and PRs
5. Never push to main/develop without permission

## Changelog

### 2025-11-13
- Created CLAUDE.md with comprehensive guidelines for AI assistants
- Documented repository state and structure
- Established development workflows and conventions

---

**Note:** This document should be updated as the project evolves. Keep it current with actual project structure, workflows, and conventions.
