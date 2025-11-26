# Contributing to TeslaMate Grafana

First off, thank you for considering contributing to TeslaMate Grafana! It's people like you that make this project better for everyone.

## Table of Contents

- [Code of Conduct](#code-of-conduct)
- [How Can I Contribute?](#how-can-i-contribute)
  - [Reporting Bugs](#reporting-bugs)
  - [Suggesting Enhancements](#suggesting-enhancements)
  - [Adding Custom Dashboards](#adding-custom-dashboards)
  - [Improving Documentation](#improving-documentation)
- [Development Setup](#development-setup)
- [Pull Request Process](#pull-request-process)
- [Style Guidelines](#style-guidelines)

---

## Code of Conduct

This project and everyone participating in it is governed by common sense and respect. By participating, you are expected to uphold this code. Please report unacceptable behavior to the repository maintainers.

---

## How Can I Contribute?

### Reporting Bugs

Before creating bug reports, please check the existing issues to avoid duplicates. When creating a bug report, include as many details as possible:

**Bug Report Template:**

```markdown
**Describe the bug**
A clear and concise description of what the bug is.

**To Reproduce**
Steps to reproduce the behavior:
1. Run command '...'
2. Navigate to '...'
3. See error

**Expected behavior**
A clear description of what you expected to happen.

**Environment:**
- Docker version: [e.g. 24.0.5]
- Architecture: [e.g. amd64, arm64]
- Image tag: [e.g. latest, 12.1.1]
- TeslaMate version: [e.g. 1.28.0]

**Additional context**
Add any other context about the problem here.
```

### Suggesting Enhancements

Enhancement suggestions are tracked as GitHub issues. When creating an enhancement suggestion, include:

- **Clear title** - Use a clear and descriptive title
- **Detailed description** - Provide a step-by-step description of the suggested enhancement
- **Use cases** - Explain why this enhancement would be useful
- **Examples** - Provide specific examples to demonstrate the enhancement

### Adding Custom Dashboards

We love community contributions of dashboards! Here's how to add yours:

1. **Fork the repository**

```bash
git clone https://github.com/YOUR-USERNAME/teslamate-grafana.git
cd teslamate-grafana
```

2. **Create a new folder for your dashboards** (optional)

```bash
mkdir -p dashboards/your-username
```

3. **Add your dashboard JSON files**

Place your exported Grafana dashboard JSON files in the appropriate folder:
- Official-style dashboards → `dashboards/`
- Custom collection → `dashboards/your-username/`

4. **Update `dashboards.yml`**

Add a new provider for your dashboard collection:

```yaml
- name: teslamate_yourusername
  type: file
  disableDeletion: false
  allowUiUpdates: true
  updateIntervalSeconds: 86400
  options:
    path: /dashboards_yourusername
    foldersFromFilesStructure: true
```

5. **Update the Dockerfile**

Add a COPY instruction for your dashboards:

```dockerfile
COPY dashboards/your-username/*.json /dashboards_yourusername/
```

6. **Update the README.md**

Add your dashboard collection to the README with descriptions:

```markdown
### 🎨 Your Name Collection (X)
Brief description of your dashboards:

- Dashboard 1 Name
- Dashboard 2 Name
- ...
```

7. **Test your changes**

```bash
docker build -t teslamate-grafana:test .
docker run -d -p 3000:3000 \
  -e DATABASE_HOST=your-db \
  -e DATABASE_USER=teslamate \
  -e DATABASE_PASS=password \
  -e DATABASE_NAME=teslamate \
  teslamate-grafana:test
```

8. **Submit a Pull Request**

### Improving Documentation

Documentation improvements are always welcome! This includes:

- Fixing typos or grammar
- Adding examples
- Clarifying existing documentation
- Adding missing documentation
- Translating documentation to other languages

---

## Development Setup

### Prerequisites

- Docker 20.10+
- Docker Buildx (for multi-architecture builds)
- Git
- A running TeslaMate instance with PostgreSQL (for testing)

### Local Development

1. **Clone the repository**

```bash
git clone https://github.com/crstian19/teslamate-grafana.git
cd teslamate-grafana
```

2. **Build the image locally**

```bash
docker build -t teslamate-grafana:dev .
```

3. **Run with your TeslaMate database**

```bash
docker run -d \
  --name teslamate-grafana-dev \
  -p 3000:3000 \
  -e DATABASE_HOST=your-postgres-host \
  -e DATABASE_USER=teslamate \
  -e DATABASE_PASS=your-password \
  -e DATABASE_NAME=teslamate \
  teslamate-grafana:dev
```

4. **Test your changes**

- Access Grafana at http://localhost:3000
- Verify all dashboards load correctly
- Check for any console errors
- Test dashboard functionality

### Multi-Architecture Testing

To test multi-architecture builds:

```bash
docker buildx create --use --name multiarch
docker buildx build \
  --platform linux/amd64,linux/arm64,linux/arm/v7 \
  -t teslamate-grafana:test \
  --load .
```

---

## Pull Request Process

1. **Create a feature branch**

```bash
git checkout -b feature/your-feature-name
# or
git checkout -b fix/your-bug-fix
```

2. **Make your changes**

- Follow the [Style Guidelines](#style-guidelines)
- Test your changes thoroughly
- Update documentation if needed

3. **Commit your changes**

Use conventional commits format:

```bash
git commit -m "feat: add new dashboard for battery statistics"
git commit -m "fix: correct datasource configuration"
git commit -m "docs: update installation instructions"
```

**Commit types:**
- `feat:` - New feature
- `fix:` - Bug fix
- `docs:` - Documentation changes
- `chore:` - Maintenance tasks
- `refactor:` - Code refactoring
- `test:` - Adding or updating tests
- `ci:` - CI/CD changes

4. **Push to your fork**

```bash
git push origin feature/your-feature-name
```

5. **Open a Pull Request**

- Use a clear and descriptive title
- Reference any related issues
- Provide a detailed description of changes
- Include screenshots for UI changes
- List any breaking changes

**PR Template:**

```markdown
## Description
Brief description of what this PR does.

## Type of Change
- [ ] Bug fix
- [ ] New feature
- [ ] Documentation update
- [ ] Dashboard addition
- [ ] Other (please describe)

## Testing
Describe how you tested your changes.

## Screenshots (if applicable)
Add screenshots to help explain your changes.

## Checklist
- [ ] My code follows the style guidelines
- [ ] I have tested my changes
- [ ] I have updated the documentation
- [ ] My commits follow the conventional commits format
- [ ] I have added my dashboards to the README (if applicable)
```

6. **Respond to feedback**

- Be open to suggestions
- Make requested changes promptly
- Keep the conversation respectful

---

## Style Guidelines

### Dashboard Guidelines

When creating or modifying dashboards:

1. **Naming Conventions**
   - Use descriptive names (e.g., "Battery Health" not "Dashboard 1")
   - Use title case for dashboard titles
   - Use lowercase with hyphens for file names (e.g., `battery-health.json`)

2. **Datasource**
   - Always use the `TeslaMate` datasource variable
   - Don't hardcode datasource IDs

3. **Variables**
   - Reuse existing dashboard variables when possible
   - Document custom variables in dashboard description

4. **Panels**
   - Use clear and descriptive panel titles
   - Add descriptions to complex panels
   - Use appropriate visualizations for data types
   - Ensure panels are responsive

5. **JSON Formatting**
   - Dashboards should be exported with "Export for sharing externally"
   - Remove `id` field from JSON (set to `null`)
   - Remove `version` field
   - Set `uid` to `null` or a unique value

### Dockerfile Guidelines

- Use official Grafana base image
- Group related environment variables
- Add comments for complex configurations
- Keep COPY instructions organized by type
- Maintain alphabetical order when possible

### Documentation Guidelines

- Use clear and concise language
- Provide examples where applicable
- Use proper markdown formatting
- Include code blocks with syntax highlighting
- Keep line length under 120 characters

---

## Questions?

Feel free to:
- Open an issue for questions
- Start a discussion in GitHub Discussions
- Reach out to the maintainers

---

## Recognition

Contributors will be recognized in:
- The project README
- Release notes for significant contributions
- GitHub's contributor graph

Thank you for contributing to TeslaMate Grafana!
