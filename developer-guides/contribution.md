# Contributing to AI Tale

## Introduction

Thank you for your interest in contributing to AI Tale! This guide will help you understand how you can contribute to our project, whether you're fixing bugs, improving documentation, or adding new features.

AI Tale is an open platform that benefits from community contributions. We welcome developers, writers, designers, and testers who want to help make AI Tale better for everyone.

## Code of Conduct

By participating in this project, you agree to abide by our [Code of Conduct](../CODE_OF_CONDUCT.md). Please read it before contributing.

## Ways to Contribute

There are many ways to contribute to AI Tale:

### Reporting Bugs

If you find a bug in the platform, please report it by creating an issue in our [GitHub repository](https://github.com/ai-tale/docs/issues). When reporting bugs, please include:

- A clear, descriptive title
- Steps to reproduce the issue
- Expected behavior vs. actual behavior
- Screenshots or code snippets if applicable
- Environment information (browser, OS, etc.)

### Suggesting Enhancements

We welcome suggestions for new features or improvements. To suggest an enhancement:

1. Check existing issues to see if your idea has already been suggested
2. Create a new issue with the label "enhancement"
3. Clearly describe the feature and its benefits
4. Include any relevant mockups or examples

### Improving Documentation

Documentation improvements are always welcome. If you find errors, confusing explanations, or missing information:

1. Fork the repository
2. Make your changes
3. Submit a pull request with a clear description of the improvements

### Contributing Code

If you'd like to contribute code to fix bugs or add features:

1. Find an issue to work on or create a new one
2. Comment on the issue to let others know you're working on it
3. Fork the repository
4. Create a feature branch
5. Make your changes
6. Submit a pull request

## Development Setup

To set up your development environment for contributing to AI Tale:

### Prerequisites

- Node.js (v14 or later)
- Python 3.8 or later
- Git

### Setting Up the Repository

1. Fork the repository on GitHub
2. Clone your fork locally:
   ```bash
   git clone https://github.com/YOUR-USERNAME/docs.git
   cd docs
   ```

3. Add the upstream repository as a remote:
   ```bash
   git remote add upstream https://github.com/ai-tale/docs.git
   ```

4. Install dependencies:
   ```bash
   # For JavaScript/Node.js components
   npm install
   
   # For Python components
   pip install -r requirements.txt
   ```

## Development Workflow

### Branching Strategy

We use a simplified Git flow with the following branches:

- `main`: The production branch, always stable
- `develop`: The development branch where features are integrated
- Feature branches: Created from `develop` for specific features or fixes

When working on a new feature or fix:

1. Create a new branch from `develop`:
   ```bash
   git checkout develop
   git pull upstream develop
   git checkout -b feature/your-feature-name
   ```

2. Make your changes, committing regularly with clear messages

3. Push your branch to your fork:
   ```bash
   git push origin feature/your-feature-name
   ```

4. Create a pull request to the `develop` branch of the main repository

### Commit Messages

We follow conventional commit messages to make the project history clear and generate changelogs automatically:

```
<type>(<scope>): <subject>

<body>

<footer>
```

Types include:
- `feat`: A new feature
- `fix`: A bug fix
- `docs`: Documentation changes
- `style`: Code style changes (formatting, etc.)
- `refactor`: Code changes that neither fix bugs nor add features
- `test`: Adding or improving tests
- `chore`: Changes to the build process or auxiliary tools

Example:
```
feat(story-generation): add support for custom character traits

This adds the ability to specify custom character traits when generating stories,
giving users more control over character development.

Closes #123
```

## Pull Request Process

1. Update the README.md or documentation with details of your changes if needed
2. Make sure your code follows our style guidelines
3. Ensure all tests pass
4. Get at least one code review from a maintainer
5. Once approved, a maintainer will merge your PR

### Pull Request Template

When creating a pull request, please use our template which includes:

- A description of the changes
- Related issue number(s)
- Type of change (bug fix, new feature, etc.)
- Checklist of completed items

## Code Style Guidelines

### JavaScript/TypeScript

We follow the [Airbnb JavaScript Style Guide](https://github.com/airbnb/javascript) with some modifications. Key points:

- Use 2 spaces for indentation
- Use semicolons
- Use single quotes for strings
- Use ES6+ features when appropriate
- Use meaningful variable and function names

### Python

We follow [PEP 8](https://www.python.org/dev/peps/pep-0008/) with some modifications. Key points:

- Use 4 spaces for indentation
- Maximum line length of 88 characters (following Black defaults)
- Use meaningful variable and function names
- Use docstrings for functions and classes

### Markdown

For documentation:

- Use ATX-style headers (`#` for titles, not underlines)
- Use fenced code blocks with language specification
- Use reference-style links when appropriate
- Line wrap at 80-100 characters when possible

## Testing Guidelines

We value tests highly and aim for good test coverage. When contributing code:

- Write unit tests for new features
- Ensure existing tests pass
- Consider edge cases in your tests
- For bug fixes, add a test that would have caught the bug

### Running Tests

For JavaScript/Node.js components:
```bash
npm test
```

For Python components:
```bash
python -m pytest
```

## Documentation Guidelines

Good documentation is crucial for our project. When contributing documentation:

- Use clear, concise language
- Include examples where appropriate
- Structure content with appropriate headings
- Update table of contents if necessary
- Check spelling and grammar

## Review Process

All contributions go through a review process:

1. Automated checks (linting, tests) run on your PR
2. At least one maintainer reviews your changes
3. You may need to make additional changes based on feedback
4. Once approved, your PR will be merged

## Community

Join our community channels to get help and discuss contributions:

- [Discord Server](https://discord.gg/aitale)
- [Community Forum](https://community.aitale.tech)
- [Twitter](https://twitter.com/aitalehq)

## Recognition

We value all contributions and recognize contributors in several ways:

- All contributors are listed in our [CONTRIBUTORS.md](../CONTRIBUTORS.md) file
- Significant contributions may be highlighted in release notes
- Regular contributors may be invited to join the core team

## License

By contributing to AI Tale, you agree that your contributions will be licensed under the project's [MIT License](../LICENSE).

## Questions?

If you have any questions about contributing, please reach out to us at contributors@aitale.tech or open an issue with your question.

Thank you for contributing to AI Tale! Your efforts help make AI-generated storytelling accessible and enjoyable for everyone.