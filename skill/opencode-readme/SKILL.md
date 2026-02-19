---
name: opencode-readme
description: README generation and maintenance for projects
---

# OpenCode README Skill

## What I Do
- Generate comprehensive README files
- Maintain existing documentation
- Extract project metadata automatically
- Create installation guides
- Document API endpoints and features
- Generate usage examples
- Update badges and shields

## When to Use
Use when:
- User asks to generate a README
- New project needs documentation
- README needs updating
- Installation guide is requested
- Project overview is needed
- Examples need to be documented

## README Sections

### 1. Project Header
```markdown
# Project Name

![Build Status](https://img.shields.io/badge/build-passing-brightgreen)
![Version](https://img.shields.io/badge/version-1.0.0-blue)
![License](https://img.shields.io/badge/license-MIT-green)

> Brief description of what the project does
```

### 2. Table of Contents
```markdown
- [Features](#features)
- [Installation](#installation)
- [Usage](#usage)
- [API Reference](#api-reference)
- [Contributing](#contributing)
- [License](#license)
```

### 3. Features
```markdown
## Features

- ✅ Feature 1 - Description
- ✅ Feature 2 - Description
- ✅ Feature 3 - Description
```

### 4. Installation
```markdown
## Installation

### Prerequisites
- Node.js >= 18
- npm >= 9

### Install
```bash
npm install project-name
```

### Development Setup
```bash
git clone https://github.com/user/repo.git
cd repo
npm install
npm run dev
```
```

### 5. Usage
```markdown
## Usage

### Basic Example
```javascript
import { Project } from 'project-name';

const app = new Project();
app.start();
```

### Advanced Configuration
```javascript
const config = {
  option1: 'value',
  option2: 123
};

const app = new Project(config);
```
```

### 6. API Reference
```markdown
## API Reference

### `method(param1, param2)`

Description of what the method does.

**Parameters:**
- `param1` (string): Description
- `param2` (number): Description

**Returns:** Promise\<Result\>

**Example:**
```javascript
await app.method('value', 123);
```
```

### 7. Configuration
```markdown
## Configuration

Create a `.config.js` file:

```javascript
module.exports = {
  apiKey: 'your-key',
  debug: true
};
```
```

### 8. Contributing
```markdown
## Contributing

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## Development

```bash
npm install
npm run dev
npm run test
npm run lint
```
```

### 9. Testing
```markdown
## Testing

```bash
# Run all tests
npm test

# Run with coverage
npm run test:coverage

# Run specific test
npm test -- --grep "specific test"
```
```

### 10. Deployment
```markdown
## Deployment

### Production
```bash
npm run build
npm start
```

### Docker
```bash
docker build -t project-name .
docker run -p 3000:3000 project-name
```
```

### 11. Troubleshooting
```markdown
## Troubleshooting

### Issue: Error message
**Solution:** Step-by-step fix

### Issue: Another problem
**Solution:** Another fix
```

### 12. License
```markdown
## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
```

## README Generation Workflow

### 1. Project Analysis
```
1. Detect project type (Node.js, Python, Go, etc.)
2. Identify package manager (npm, yarn, pip, etc.)
3. Extract dependencies from package files
4. Find entry points and main files
5. Identify frameworks and libraries used
```

### 2. Documentation Extraction
```
1. Parse JSDoc/DocBlock comments
2. Extract API endpoints (REST/GraphQL)
3. Find configuration files
4. Identify environment variables
5. Discover test patterns
```

### 3. Content Generation
```
1. Generate badges based on CI/CD status
2. Create installation instructions
3. Document usage from code examples
4. Extract API documentation
5. Build configuration examples
```

### 4. Validation
```
1. Verify all code examples are valid
2. Test installation commands
3. Check links and references
4. Ensure consistent formatting
5. Validate badges are working
```

## Project Type Detection

### Node.js Projects
- package.json analysis
- npm/yarn/pnpm detection
- Framework detection (Express, React, Next.js, etc.)
- Build tools (webpack, vite, rollup)

### Python Projects
- setup.py / pyproject.toml / requirements.txt
- pip/poetry detection
- Framework detection (Django, Flask, FastAPI)
- Virtual environment setup

### Go Projects
- go.mod analysis
- Go version detection
- Dependency management
- Build instructions

### Java Projects
- pom.xml / build.gradle
- Maven/Gradle detection
- Framework detection (Spring Boot, Jakarta EE)
- JAR/WAR build process

## Badge Templates

### CI/CD Status
```markdown
![Build](https://img.shields.io/github/actions/workflow/ci.yml/user/repo)
![Tests](https://img.shields.io/coveralls/github/user/repo)
```

### Version and License
```markdown
![Version](https://img.shields.io/badge/version-1.0.0-blue)
![License](https://img.shields.io/badge/license-MIT-green)
```

### Downloads and Popularity
```markdown
![Downloads](https://img.shields.io/npm/dw/package-name)
![Stars](https://img.shields.io/github/stars/user/repo)
```

### Code Quality
```markdown
![Code Quality](https://img.shields.io/codacy/grade/user/repo)
![Coverage](https://img.shields.io/codecov/c/github/user/repo)
```

## README Maintenance

### Update Triggers
- New version release
- New API endpoints added
- Dependencies updated
- Breaking changes introduced
- New features added

### Update Checklist
- [ ] Update version badges
- [ ] Document new features
- [ ] Update installation instructions
- [ ] Add new API documentation
- [ ] Update examples
- [ ] Fix broken links
- [ ] Update screenshots/demos

## Best Practices

1. **Keep it current** - Update with every release
2. **Clear examples** - Provide working code samples
3. **Visual aids** - Use diagrams, screenshots, GIFs
4. **Link everything** - Cross-reference sections
5. **Multiple languages** - Show examples in different languages
6. **Accessibility** - Use semantic markdown
7. **Searchable** - Include keywords for discoverability

## Safety Pattern

1. **Read-only generation** - Don't modify source files
2. **Verify examples** - Test code snippets
3. **Backup existing** - Keep old README
4. **Draft first** - Show before saving
5. **User review** - Get approval before overwriting

## Example Prompts

```
"Generate README for this project"
"Create comprehensive documentation"
"Update README with new features"
"Add installation guide for Python"
"Document API endpoints in README"
"Add badges and shields to README"
```

## Output Format

```markdown
# [Project Name]

[Badges]

## Description
[What the project does]

## Features
- Feature 1
- Feature 2

## Installation
[Step-by-step instructions]

## Usage
[Code examples]

## API Reference
[Documentation]

## Contributing
[Guidelines]

## License
[License info]
```
