# Pull Request

## Description

### Summary

Provide a clear and concise summary of what this PR does.

### Motivation

Why is this change needed? What problem does it solve?

Fixes #(issue number)
Closes #(issue number)

## Type of Change

- [ ] Bug fix (non-breaking change which fixes an issue)
- [ ] New feature (non-breaking change which adds functionality)
- [ ] Breaking change (fix or feature that would cause existing functionality to not work as expected)
- [ ] Documentation update
- [ ] Code refactoring (no functional changes)
- [ ] Performance improvement
- [ ] Test addition or update
- [ ] CI/CD improvement
- [ ] Dependency update

## Changes Made

### Detailed Changes

Provide a detailed list of changes made:

- Added X to support Y
- Modified Z to improve performance
- Fixed bug in A that caused B
- Updated documentation for C

### Files Changed

**Core changes:**
- `src/nuevo_proyecto_scaffold/wizard.py` - Added new phase
- `src/nuevo_proyecto_scaffold/generator.py` - Template generation logic

**Template changes:**
- `templates/backend/fastapi/...` - New template files
- `templates/shared/...` - Shared configuration updates

**Documentation:**
- `docs/USAGE.md` - Usage documentation
- `docs/WIZARD-PHASES.md` - Phase documentation

**Tests:**
- `tests/test_wizard.py` - Test coverage for new phase
- `tests/test_generator.py` - Template generation tests

## Testing

### Test Strategy

Describe how you tested this change:

**Manual testing:**
```bash
# Commands used to test
uv run python -m nuevo_proyecto_scaffold.wizard
cd generated-project
docker compose up -d
# ... test steps
```

**Automated tests:**
```bash
uv run pytest -v
uv run pytest --cov=src
```

### Test Results

**All tests passing:** Yes/No

**Coverage:**
- Previous: X%
- Current: Y%
- Change: +/- Z%

### Test Scenarios Covered

- [ ] Wizard completes successfully with new options
- [ ] Generated project structure is correct
- [ ] Generated code compiles/runs without errors
- [ ] Docker containers start successfully
- [ ] Tests in generated project pass
- [ ] CI/CD workflows run successfully
- [ ] Documentation is accurate

### Generated Project Testing

**If this PR changes generated code:**

**Test project generated with:**
- Scale: (Micro/Small/Medium/Large/Enterprise)
- Backend: (FastAPI/NestJS/API-less)
- Frontend: (Next.js/None)
- Other relevant options: ...

**Verification steps performed:**
1. Generated project with above configuration
2. Ran `docker compose up -d`
3. Verified all services started
4. Ran tests: `uv run pytest` (or `npm test`)
5. Manually tested core functionality
6. Checked for errors in logs

**Results:** All checks passed / Issues found (describe)

## Breaking Changes

### Is this a breaking change?

- [ ] No
- [ ] Yes (describe below)

### If breaking, describe the impact:

**What breaks:**

**Migration path:**

**Affected versions:**

## Screenshots / Examples

### Before

(If applicable, show the previous behavior)

### After

(Show the new behavior)

### Generated Code Example

```python
# Example of generated code from this PR
# Show key snippets that demonstrate the changes
```

## Checklist

### Code Quality

- [ ] My code follows the project's style guidelines
- [ ] I have performed a self-review of my own code
- [ ] I have commented my code, particularly in hard-to-understand areas
- [ ] My changes generate no new warnings
- [ ] I have added author attribution to new files
- [ ] Code is properly formatted (ran `uv run ruff format .`)
- [ ] No linting errors (ran `uv run ruff check --fix .`)
- [ ] Type checking passes (ran `uv run mypy src/`)

### Testing

- [ ] I have added tests that prove my fix is effective or that my feature works
- [ ] New and existing unit tests pass locally with my changes
- [ ] I have tested the generated project manually
- [ ] Test coverage has not decreased

### Documentation

- [ ] I have updated the documentation accordingly
- [ ] I have updated `docs/USAGE.md` if needed
- [ ] I have updated `docs/WIZARD-PHASES.md` if adding/modifying phases
- [ ] I have updated the README if needed
- [ ] I have added docstrings to new functions/classes
- [ ] Comments are clear and helpful

### Commits

- [ ] My commits follow the conventional commits specification
- [ ] Commit messages are clear and descriptive
- [ ] I have squashed unnecessary commits

### Templates (if applicable)

- [ ] New templates follow project structure conventions
- [ ] Templates generate valid, working code
- [ ] Templates include proper error handling
- [ ] Templates follow language-specific best practices
- [ ] Template variables are properly documented

### Integration

- [ ] This PR is based on the latest main branch
- [ ] I have resolved any merge conflicts
- [ ] CI/CD checks pass

## Additional Notes

### Dependencies

**New dependencies added:**
- Package name: version (reason)

**Dependencies removed:**
- Package name (reason)

**Dependencies updated:**
- Package name: old version → new version (reason)

### Performance Impact

- [ ] No performance impact
- [ ] Performance improvement (describe)
- [ ] Potential performance concern (describe and justify)

### Security Considerations

- [ ] No security implications
- [ ] Security improvement (describe)
- [ ] Requires security review (describe concerns)

### Future Work

**Follow-up tasks (if any):**
- [ ] Task 1
- [ ] Task 2

**Known limitations:**
- Limitation 1
- Limitation 2

## Reviewer Notes

### Areas to Focus On

Please pay special attention to:
1. Area 1 - reason
2. Area 2 - reason

### Questions for Reviewers

- Question 1?
- Question 2?

## Related Issues / PRs

- Related to #(issue)
- Depends on #(PR)
- Blocked by #(PR)

---

**Ready for review:** Yes/No

**Reviewers:** @username1 @username2
