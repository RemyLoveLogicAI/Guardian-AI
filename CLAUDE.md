# CLAUDE.md - AI Assistant Guide for Guardian-AI

This document provides comprehensive guidance for AI assistants (like Claude) working on the Guardian-AI project.

## Project Overview

**Guardian-AI** is an AI-powered project currently in its early development phase. This repository is being actively developed with CI/CD automation via CircleCI.

### Repository Information
- **Owner**: RemyLoveLogicAI
- **Repository**: Guardian-AI
- **Primary Branch**: `main` (or master)
- **Remote**: http://local_proxy@127.0.0.1:20482/git/RemyLoveLogicAI/Guardian-AI

## Repository Structure

```
Guardian-AI/
├── .circleci/           # CircleCI configuration
│   └── config.yml       # CI/CD pipeline definition
├── .git/                # Git metadata
└── CLAUDE.md            # This file - AI assistant guidelines
```

### Future Expected Structure

As the project grows, expect the following structure to emerge:

```
Guardian-AI/
├── .circleci/           # CI/CD configuration
├── src/                 # Source code
├── tests/               # Test files
├── docs/                # Documentation
├── scripts/             # Build and deployment scripts
├── config/              # Configuration files
├── package.json         # Node.js dependencies (if applicable)
├── requirements.txt     # Python dependencies (if applicable)
├── README.md            # Project documentation
├── CONTRIBUTING.md      # Contribution guidelines
├── LICENSE              # License information
└── CLAUDE.md            # This file
```

## Development Workflow

### Branch Strategy

1. **Feature Branches**: All development work happens on feature branches
   - Branch naming: `claude/<description>-<sessionId>`
   - Example: `claude/add-claude-documentation-oBfjS`

2. **Main Branch**: Protected branch containing production-ready code
   - Never push directly to main
   - All changes must go through Pull Requests

### Git Practices

#### Branching
```bash
# Create and switch to a new feature branch
git checkout -b claude/<feature-name>-<sessionId>

# Switch to existing branch
git checkout <branch-name>
```

#### Committing
```bash
# Stage changes
git add <files>

# Commit with descriptive message
git commit -m "Clear, descriptive commit message"
```

**Commit Message Conventions**:
- Use present tense: "Add feature" not "Added feature"
- Be specific: "Add user authentication module" not "Update code"
- Reference issues if applicable: "Fix #123: Resolve login timeout"
- Keep first line under 72 characters

#### Pushing
```bash
# Always push with -u flag to set upstream
git push -u origin <branch-name>

# CRITICAL: Branch must start with 'claude/' and end with session ID
# Otherwise push will fail with 403 error
```

**Push Retry Logic**:
- If push fails due to network errors, retry up to 4 times
- Use exponential backoff: 2s, 4s, 8s, 16s between retries

#### Fetching and Pulling
```bash
# Fetch specific branch
git fetch origin <branch-name>

# Pull specific branch
git pull origin <branch-name>

# Apply same retry logic (4 retries with exponential backoff)
```

### Pull Request Process

1. **Commit** all changes with clear messages
2. **Push** to your feature branch
3. **Create PR** using GitHub CLI:
```bash
gh pr create --title "Title" --body "$(cat <<'EOF'
## Summary
- Bullet point summary of changes

## Test Plan
- [ ] Tests added/updated
- [ ] Manual testing performed
- [ ] CI pipeline passes
EOF
)"
```

## CI/CD Configuration

### CircleCI Setup

The project uses CircleCI for continuous integration. Current configuration (.circleci/config.yml:1-31):

- **Version**: CircleCI 2.1
- **Executor**: Docker with `cimg/base:current`
- **Current Jobs**:
  - `say-hello`: Basic hello world job

**Expected Future Jobs**:
- Linting and code quality checks
- Unit tests
- Integration tests
- Build verification
- Deployment steps

### Adding New CI Jobs

When adding CI jobs, follow this pattern:

```yaml
jobs:
  job-name:
    docker:
      - image: cimg/base:current
    steps:
      - checkout
      - run:
          name: "Descriptive name"
          command: "command to run"

workflows:
  workflow-name:
    jobs:
      - job-name
```

## Coding Conventions

### General Principles

1. **Code Quality**
   - Write clean, readable, maintainable code
   - Follow DRY (Don't Repeat Yourself) principle
   - Keep functions small and focused
   - Use meaningful variable and function names

2. **Documentation**
   - Document complex logic with comments
   - Keep comments up-to-date with code changes
   - Write clear docstrings for functions/classes
   - Update README for user-facing changes

3. **Error Handling**
   - Implement proper error handling
   - Use try-catch blocks appropriately
   - Log errors with sufficient context
   - Provide meaningful error messages

### Security Best Practices

⚠️ **Critical Security Guidelines**:

- **Never commit secrets**: No API keys, passwords, tokens in code
- **Validate all input**: Sanitize user input to prevent injection attacks
- **Use parameterized queries**: Prevent SQL injection
- **Escape output**: Prevent XSS vulnerabilities
- **Keep dependencies updated**: Regularly update to patch vulnerabilities
- **Follow OWASP Top 10**: Be aware of common security vulnerabilities

### Code Review Checklist

Before committing, verify:
- [ ] Code follows project conventions
- [ ] No hardcoded secrets or sensitive data
- [ ] Error handling is appropriate
- [ ] Tests are included/updated
- [ ] Documentation is updated
- [ ] No unnecessary console.logs or debug code
- [ ] Code is formatted consistently

## Testing Strategy

### Test Types

1. **Unit Tests**: Test individual functions/components
2. **Integration Tests**: Test component interactions
3. **End-to-End Tests**: Test complete user flows

### Testing Best Practices

- Write tests before or alongside code (TDD/BDD)
- Aim for high code coverage (80%+ recommended)
- Test edge cases and error conditions
- Keep tests fast and independent
- Use descriptive test names

### Running Tests

```bash
# Run all tests (command TBD based on tech stack)
npm test          # For Node.js projects
pytest            # For Python projects

# Run specific test file
npm test <file>
pytest <file>

# Run with coverage
npm test -- --coverage
pytest --cov
```

## AI Assistant Guidelines

### Task Approach

1. **Read Before Modifying**
   - Always read files before suggesting changes
   - Understand existing code structure
   - Respect established patterns

2. **Use TodoWrite Tool**
   - Plan complex tasks with TodoWrite
   - Track progress transparently
   - Mark tasks complete immediately upon finishing

3. **Ask Questions**
   - Use AskUserQuestion for clarification
   - Confirm architectural decisions
   - Validate assumptions

### Tool Usage

**Prefer Specialized Tools**:
- `Read`: For reading files (not `cat`)
- `Edit`: For modifying files (not `sed/awk`)
- `Write`: For creating files (not `echo >`)
- `Grep`: For searching code (not bash `grep`)
- `Glob`: For finding files (not `find`)

**Bash Tool**: Reserve for actual shell operations
- Git commands
- Package manager commands (npm, pip, etc.)
- Build tools
- System operations

**Never Use Bash For**:
- Reading files
- Searching code
- Creating/editing files
- Communicating with users

### Code References

When referencing code, use the format: `file_path:line_number`

Example: "The CircleCI configuration is defined in `.circleci/config.yml:1-31`"

### Commit Workflow

**Git Safety Protocol**:
- ✅ NEVER update git config
- ✅ NEVER run destructive commands (force push, hard reset) without explicit approval
- ✅ NEVER skip hooks (--no-verify) without explicit approval
- ✅ NEVER force push to main/master
- ✅ Avoid `git commit --amend` unless explicitly requested
- ✅ Only commit when user explicitly requests it

**Creating Commits**:

1. Run git status and git diff in parallel
2. Analyze changes and draft commit message
3. Add files and commit (use HEREDOC for messages)
4. Run git status to verify success

Example:
```bash
git commit -m "$(cat <<'EOF'
Add comprehensive CLAUDE.md documentation

This commit adds detailed guidelines for AI assistants including:
- Repository structure and conventions
- Development workflow and git practices
- CI/CD configuration details
- Security best practices
EOF
)"
```

### Avoid Over-Engineering

- Only make requested changes
- Don't add unrequested features
- Don't refactor surrounding code unnecessarily
- Keep solutions simple and focused
- No premature abstractions

### Communication Style

- Be concise and direct
- Use markdown formatting
- No emojis unless requested
- Focus on facts, not validation
- Output text directly (not via echo/printf)

## Project-Specific Notes

### Current State

This is a **new project** in early development:
- Minimal codebase (only CircleCI config exists)
- No dependencies defined yet
- No test framework set up yet
- Technology stack to be determined

### Next Steps for Development

When adding code to this project:

1. **Define the tech stack**: Decide on programming language(s) and frameworks
2. **Set up project structure**: Create src/, tests/, docs/ directories
3. **Initialize package management**: Add package.json, requirements.txt, etc.
4. **Configure linting**: Add ESLint, Pylint, or equivalent
5. **Add testing framework**: Jest, Pytest, or equivalent
6. **Expand CI/CD**: Add real build, test, and lint jobs
7. **Create README**: Document project purpose and setup
8. **Add LICENSE**: Choose and add appropriate license

### Questions to Clarify

Before significant development, clarify:
- What is the primary purpose of Guardian-AI?
- What programming language(s) will be used?
- What frameworks or libraries are preferred?
- What is the target deployment platform?
- What are the security requirements?
- What are the performance requirements?

## Maintenance

### Updating This Document

This document should be updated when:
- Project structure changes significantly
- New conventions are established
- New tools or workflows are adopted
- Security practices evolve
- CI/CD pipeline is modified

### Version History

- **2026-01-18**: Initial creation with comprehensive AI assistant guidelines

## References

### Internal Documentation
- `.circleci/config.yml`: CI/CD pipeline configuration

### External Resources
- [CircleCI Documentation](https://circleci.com/docs/)
- [Git Best Practices](https://git-scm.com/book/en/v2)
- [OWASP Top 10](https://owasp.org/www-project-top-ten/)
- [Semantic Versioning](https://semver.org/)

---

**Note**: This document is a living guide and should be updated as the project evolves. AI assistants should always refer to this document before making significant changes to the codebase.
