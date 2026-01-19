# Contributing to Nuevo Proyecto Scaffold

Thank you for your interest in contributing to Nuevo Proyecto Scaffold! This document provides guidelines and instructions for contributing to this project.

## Table of Contents

- [Code of Conduct](#code-of-conduct)
- [Getting Started](#getting-started)
- [Development Setup](#development-setup)
- [How to Contribute](#how-to-contribute)
- [Code Style Guidelines](#code-style-guidelines)
- [Commit Message Format](#commit-message-format)
- [Pull Request Process](#pull-request-process)
- [Template Structure](#template-structure)
- [Adding New Templates](#adding-new-templates)
- [Testing Requirements](#testing-requirements)
- [Code Review Checklist](#code-review-checklist)

## Code of Conduct

This project follows a code of conduct to ensure a welcoming environment for all contributors. Please be respectful, constructive, and professional in all interactions.

## Getting Started

1. Fork the repository
2. Clone your fork: `git clone https://github.com/YOUR_USERNAME/nuevo-proyecto-scaffold.git`
3. Add upstream remote: `git remote add upstream https://github.com/ORIGINAL_OWNER/nuevo-proyecto-scaffold.git`
4. Create a feature branch: `git checkout -b feature/your-feature-name`

## Development Setup

### Prerequisites

- Python 3.12+
- uv (package manager): `curl -LsSf https://astral.sh/uv/install.sh | sh`
- Git

### Installation

```bash
# Navigate to project directory
cd nuevo-proyecto-scaffold

# Install dependencies
uv sync

# Install pre-commit hooks
uv run pre-commit install
```

### Running the Wizard Locally

```bash
# Run the wizard
uv run python -m nuevo_proyecto_scaffold.wizard

# Or with Python directly
python -m nuevo_proyecto_scaffold.wizard
```

## How to Contribute

### Types of Contributions

We welcome:

- **Bug fixes**: Fix issues in existing templates or wizard logic
- **New templates**: Add support for new frameworks, tools, or configurations
- **Documentation improvements**: Enhance guides, add examples, fix typos
- **Feature enhancements**: Improve wizard UX, add new phases, enhance generated code
- **Performance improvements**: Optimize template generation, reduce dependencies
- **Tests**: Add or improve test coverage

### Reporting Bugs

Use the [Bug Report template](.github/ISSUE_TEMPLATE/bug_report.md) when creating an issue. Include:

- Clear description of the issue
- Steps to reproduce
- Expected vs actual behavior
- Environment details (OS, Python version, etc.)
- Screenshots or logs if applicable

### Suggesting Features

Use the [Feature Request template](.github/ISSUE_TEMPLATE/feature_request.md). Include:

- Clear description of the feature
- Use case and motivation
- Proposed implementation (if applicable)
- Alternative solutions considered

## Code Style Guidelines

### Python Code Style

We follow PEP 8 with some modifications enforced by Ruff:

- **Line length**: 100 characters maximum
- **Imports**: Organized automatically by Ruff (stdlib, third-party, local)
- **Naming conventions**:
  - Functions/variables: `snake_case`
  - Classes: `PascalCase`
  - Constants: `UPPER_SNAKE_CASE`
  - Private attributes: `_leading_underscore`

### Type Hints

Use type hints for all public functions and methods:

```python
def generate_template(
    project_name: str,
    config: ProjectConfig,
    output_path: Path
) -> bool:
    """Generate project template from configuration.

    Args:
        project_name: Name of the project
        config: Project configuration object
        output_path: Target directory for generated files

    Returns:
        True if generation succeeded, False otherwise
    """
    ...
```

### Docstrings

Use Google-style docstrings for modules, classes, and functions:

```python
def process_template(template: str, context: dict[str, Any]) -> str:
    """Process a Jinja2 template with given context.

    Args:
        template: Template string to process
        context: Dictionary of template variables

    Returns:
        Processed template string

    Raises:
        TemplateError: If template syntax is invalid
    """
```

### File Organization

- Keep modules focused and under 300 lines
- Group related functionality
- Use `__init__.py` for package exports
- Separate concerns: wizard logic, template generation, file operations

### Author Attribution

All new files must include author attribution:

```python
"""
module_name.py - Brief description

Autor: Homero Thompson del Lago del Terror
"""
```

## Commit Message Format

We follow [Conventional Commits](https://www.conventionalcommits.org/) specification:

```
<type>(<scope>): <subject>

[optional body]

[optional footer]
```

### Types

- `feat`: New feature
- `fix`: Bug fix
- `docs`: Documentation only
- `style`: Code style changes (formatting, no logic change)
- `refactor`: Code refactoring (no feature change)
- `perf`: Performance improvement
- `test`: Add or modify tests
- `chore`: Maintenance tasks
- `ci`: CI/CD changes

### Scopes

- `wizard`: Wizard logic and phases
- `templates`: Template files and generation
- `cli`: Command-line interface
- `config`: Configuration handling
- `docs`: Documentation
- `deps`: Dependencies

### Examples

```bash
feat(wizard): add scale selection phase with recommendations

- Implement scale phase (micro, small, medium, large, enterprise)
- Add decision helper based on team size and traffic
- Integrate scale into config defaults

fix(templates): correct docker-compose port mapping for PostgreSQL

docs(usage): add troubleshooting section for common errors

refactor(generator): extract template rendering to separate module

test(wizard): add unit tests for scale phase logic
```

### Footer

Include issue references and breaking changes:

```
feat(wizard): redesign authentication phase

BREAKING CHANGE: Auth phase now returns AuthConfig object instead of dict

Fixes #123
Closes #456
```

## Pull Request Process

### Before Submitting

1. **Create an issue first** for significant changes
2. **Update tests** to cover your changes
3. **Run quality checks** locally:
   ```bash
   uv run ruff check --fix .
   uv run ruff format .
   uv run mypy src/
   uv run pytest -v --cov
   ```
4. **Update documentation** if needed
5. **Test manually** by running the wizard

### PR Checklist

Use the [PR template](.github/PULL_REQUEST_TEMPLATE.md) and ensure:

- [ ] Code follows style guidelines
- [ ] Self-review completed
- [ ] Comments added for complex logic
- [ ] Documentation updated
- [ ] Tests added/updated
- [ ] All tests pass
- [ ] No new warnings
- [ ] Commit messages follow convention
- [ ] Author attribution included in new files

### PR Description

Your PR should include:

- **Summary**: What does this PR do?
- **Motivation**: Why is this change needed?
- **Changes**: Detailed list of modifications
- **Testing**: How was this tested?
- **Screenshots**: For UI/output changes
- **Breaking changes**: If applicable
- **Related issues**: Link to issues

### Review Process

1. Automated checks run (tests, linting, type checking)
2. Code review by maintainer(s)
3. Address feedback with new commits
4. Approval and merge by maintainer

## Template Structure

### Directory Layout

```
templates/
├── shared/              # Shared files across all templates
│   ├── gitignore.j2
│   ├── pre-commit-config.j2
│   └── README.j2
├── backend/
│   ├── fastapi/         # FastAPI-specific templates
│   ├── nestjs/          # NestJS-specific templates
│   └── common/          # Common backend files
├── frontend/
│   └── nextjs/          # Next.js templates
├── infrastructure/
│   ├── docker/          # Docker configurations
│   ├── github-actions/  # CI/CD workflows
│   └── terraform/       # IaC templates (future)
└── docs/                # Documentation templates
```

### Template Files

Templates use Jinja2 syntax with `.j2` extension:

```jinja2
{# templates/backend/fastapi/main.py.j2 #}
"""
{{ project_name }} - Main application module

Autor: {{ author_name }}
"""

from fastapi import FastAPI

app = FastAPI(
    title="{{ project_name }}",
    version="0.1.0",
    {% if config.enable_docs %}
    docs_url="/docs",
    {% else %}
    docs_url=None,
    {% endif %}
)

@app.get("/health")
async def health_check():
    return {"status": "healthy"}
```

### Context Variables

Templates receive a context dictionary with:

- `project_name`: Project name
- `author_name`: Author name (default: "Homero Thompson del Lago del Terror")
- `config`: ProjectConfig object with all wizard selections
- `current_date`: Current date (YYYY-MM-DD format)
- Additional phase-specific variables

## Adding New Templates

### Step 1: Create Template Files

```bash
# Create directory for new template
mkdir -p templates/backend/express

# Add template files with .j2 extension
touch templates/backend/express/app.ts.j2
touch templates/backend/express/package.json.j2
```

### Step 2: Update Template Mapping

Edit `src/nuevo_proyecto_scaffold/generator.py`:

```python
TEMPLATE_MAP = {
    "backend": {
        "fastapi": "backend/fastapi",
        "nestjs": "backend/nestjs",
        "express": "backend/express",  # Add new template
    },
    # ...
}
```

### Step 3: Update Wizard Options

Edit the appropriate wizard phase to include the new option:

```python
# In wizard.py, update backend phase
backend_options = [
    "FastAPI (Python)",
    "NestJS (TypeScript)",
    "Express (TypeScript)",  # Add new option
]
```

### Step 4: Add Tests

Create test file `tests/test_express_template.py`:

```python
"""Tests for Express template generation."""

import pytest
from pathlib import Path
from nuevo_proyecto_scaffold.generator import generate_project

def test_express_template_generation(tmp_path: Path):
    """Test Express template generates correctly."""
    config = ProjectConfig(
        backend="express",
        # ... other config
    )

    result = generate_project(
        project_name="test-express",
        config=config,
        output_path=tmp_path
    )

    assert result is True
    assert (tmp_path / "app.ts").exists()
    assert (tmp_path / "package.json").exists()
```

### Step 5: Document the New Template

Update `docs/WIZARD-PHASES.md` with the new template option and its characteristics.

## Testing Requirements

### Test Types

1. **Unit tests**: Test individual functions and classes
2. **Integration tests**: Test template generation end-to-end
3. **Wizard tests**: Test wizard phase logic and flow

### Running Tests

```bash
# Run all tests
uv run pytest -v

# Run with coverage
uv run pytest --cov=src --cov-report=term-missing

# Run specific test file
uv run pytest tests/test_wizard.py

# Run tests matching pattern
uv run pytest -k "test_fastapi"
```

### Writing Tests

Use pytest with AAA pattern (Arrange-Act-Assert):

```python
def test_generate_fastapi_template(tmp_path: Path):
    # Arrange
    config = ProjectConfig(backend="fastapi", database="postgresql")

    # Act
    result = generate_project("test-api", config, tmp_path)

    # Assert
    assert result is True
    assert (tmp_path / "src" / "main.py").exists()
    content = (tmp_path / "src" / "main.py").read_text()
    assert "FastAPI" in content
```

### Test Coverage

- Minimum coverage: 80%
- Critical paths: 100% coverage
- Run coverage locally before submitting PR

## Code Review Checklist

### For Reviewers

- [ ] Code follows style guidelines
- [ ] Logic is clear and well-documented
- [ ] Tests are adequate and pass
- [ ] No unnecessary complexity
- [ ] Error handling is appropriate
- [ ] Performance considerations addressed
- [ ] Security implications considered
- [ ] Breaking changes documented
- [ ] Documentation updated

### For Authors (Self-Review)

Before requesting review:

- [ ] Read your own diff carefully
- [ ] Remove debug code and comments
- [ ] Check for hardcoded values
- [ ] Verify error messages are helpful
- [ ] Ensure consistent naming
- [ ] Remove unused imports/variables
- [ ] Test edge cases
- [ ] Verify generated project works

## Questions?

If you have questions or need help:

1. Check existing documentation
2. Search closed issues
3. Open a discussion on GitHub
4. Tag maintainers in your PR

## Recognition

Contributors are recognized in:

- GitHub contributors page
- Release notes for significant contributions
- Special recognition for major features

Thank you for contributing to Nuevo Proyecto Scaffold!

---

**Author:** Homero Thompson del Lago del Terror
**Last Updated:** 2026-01-19
