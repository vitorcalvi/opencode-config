# OpenCode Configuration

OpenCode configuration files, plugins, and custom skills for AI-powered development.

## Overview

This repository contains the complete OpenCode development environment configuration, including:
- Agent configurations and model settings
- Custom plugins and skills
- MCP (Model Context Protocol) server configurations
- Development environment setup

## Setup

### Prerequisites

- OpenCode CLI installed
- Node.js 18+ with Bun or npm
- Environment variables configured

### Installation

1. Clone this repository:
```bash
git clone https://github.com/vitorcalvi/opencode_ohmyopencode_config.git ~/.config/opencode
cd ~/.config/opencode
```

2. Install dependencies:
```bash
bun install
```

3. Configure environment variables:
```bash
cp .env.example .env
# Edit .env with your API keys
```

### Required Environment Variables

See `.env.example` for the complete list:
- `ZAI_API_KEY` - Z.AI API key for coding models
- `GITHUB_COPILOT_TOKEN` - GitHub Copilot token (optional)

## Configuration Files

- `opencode.json` - Main OpenCode configuration
- `oh-my-opencode.json` - Oh My OpenCode plugin settings
- `package.json` - NPM/Bun dependencies and workspaces

## Directory Structure

```
.
├── instructions/     # Custom AI instructions and prompts
├── plugin/          # OpenCode plugins
├── skill/           # Custom skills for agents
├── prompts/         # Prompt templates
├── packages/        # Shared packages
├── opencode.json    # Main configuration
└── oh-my-opencode.json  # Plugin configuration
```

## Usage

OpenCode automatically loads configurations from `~/.config/opencode`. Any changes to configuration files will be applied on restart.

## Development

### Running commands

```bash
# Development
bun run dev

# Build
bun run build

# Test
bun run test

# Lint
bun run lint

# Typecheck
bun run typecheck
```

## MCP Servers Configured

- **Filesystem** - Local filesystem access
- **Memory** - Persistent memory storage
- **Git MCP** - Git operations
- **GitHub** - GitHub API integration
- **Qdrant** - Vector database for RAG

## License

MIT

## Contributing

Contributions welcome! Please open an issue or submit a pull request.