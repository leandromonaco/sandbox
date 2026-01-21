# Getting Started

This guide will walk you through installing, configuring, and using the CLI tool for the first time.

## Prerequisites

Before you begin, make sure you have the following installed:

- **Python 3.8 or higher**: The CLI tool is built with Python
- **pip**: Python package manager (usually comes with Python)
- **Git**: For version control (optional but recommended)

Check your Python version:

```bash
python --version
# or
python3 --version
```

## Installation

### Option 1: Install from PyPI (Recommended)

Install the latest stable version using pip:

```bash
pip install cli-tool
```

Or with Python 3 explicitly:

```bash
python3 -m pip install cli-tool
```

### Option 2: Install from Source

For the latest development version:

```bash
git clone https://github.com/yourusername/cli-tool.git
cd cli-tool
pip install -e .
```

### Verify Installation

Confirm the installation was successful:

```bash
cli-tool --version
```

You should see output similar to:

```
CLI Tool version 1.0.0
```

## Configuration

### Initial Setup

Run the setup wizard to configure the CLI tool:

```bash
cli-tool setup
```

This will guide you through:

- Setting your default project directory
- Configuring cloud provider credentials (if applicable)
- Setting up default deployment options

### Configuration File

The CLI tool stores configuration in `~/.cli-tool/config.yaml`:

```yaml
# Default configuration file
default_project_dir: ~/projects
cloud_provider: aws
region: us-east-1
verbose: false
```

You can edit this file manually or use the `cli-tool config` command:

```bash
# View current configuration
cli-tool config list

# Set a configuration value
cli-tool config set region us-west-2

# Reset to defaults
cli-tool config reset
```

## Basic Usage

### Creating Your First Project

Initialize a new project:

```bash
cli-tool init my-first-project
```

This creates a new directory structure:

```
my-first-project/
├── config/
│   └── settings.yaml
├── src/
│   └── main.py
├── tests/
└── README.md
```

### Working with Environments

The CLI tool supports multiple environments:

```bash
# Set current environment
cli-tool env set development

# List available environments
cli-tool env list

# Create a new environment
cli-tool env create staging --copy-from development
```

### Running Commands

Most commands follow this pattern:

```bash
cli-tool <command> [options] [arguments]
```

Get help for any command:

```bash
cli-tool --help
cli-tool init --help
cli-tool deploy --help
```

## Common Workflows

### Workflow 1: Start a New Project

```bash
# Create and initialize project
cli-tool init my-app --template web

# Enter project directory
cd my-app

# Check status
cli-tool status
```

### Workflow 2: Deploy an Application

```bash
# Build for production
cli-tool build --env production

# Run tests
cli-tool test

# Deploy
cli-tool deploy --env production

# Verify deployment
cli-tool status --env production
```

### Workflow 3: Monitor and Troubleshoot

```bash
# Check deployment status
cli-tool status

# View logs
cli-tool logs --tail 100

# Roll back if needed
cli-tool rollback --version previous
```

## Tips and Best Practices

### Use Aliases

Create shell aliases for frequently used commands:

```bash
# Add to your .bashrc or .zshrc
alias ct="cli-tool"
alias ctd="cli-tool deploy"
alias cts="cli-tool status"
```

### Enable Tab Completion

Enable shell completion for faster command entry:

```bash
# For Bash
cli-tool completion bash > ~/.cli-tool-completion.bash
echo "source ~/.cli-tool-completion.bash" >> ~/.bashrc

# For Zsh
cli-tool completion zsh > ~/.cli-tool-completion.zsh
echo "source ~/.cli-tool-completion.zsh" >> ~/.zshrc
```

### Use Configuration Profiles

Manage multiple configurations with profiles:

```bash
# Create a work profile
cli-tool config profile create work

# Switch profiles
cli-tool config profile use work

# List profiles
cli-tool config profile list
```

## Upgrading

Keep your CLI tool up to date:

```bash
# Check for updates
cli-tool update check

# Upgrade to latest version
pip install --upgrade cli-tool
```

## Uninstallation

If you need to remove the CLI tool:

```bash
pip uninstall cli-tool
```

## Next Steps

Now that you have the CLI tool installed and configured:

- Explore available [commands](commands/init.md)
- Read the [FAQ](faq.md) for common questions
- Check out example projects in our GitHub repository
- Join our community for support and discussions

## Troubleshooting

If you encounter issues during installation:

- **Permission errors**: Try using `pip install --user cli-tool`
- **Python version issues**: Ensure you're using Python 3.8 or higher
- **Command not found**: Make sure Python's bin directory is in your PATH

For more troubleshooting help, see the [FAQ](faq.md).
