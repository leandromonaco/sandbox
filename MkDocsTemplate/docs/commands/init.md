# init Command

The `init` command initializes a new project with a standard directory structure and configuration files.

## Synopsis

```bash
cli-tool init [OPTIONS] PROJECT_NAME
```

## Description

Creates a new project directory with all necessary files and folders to get started quickly. The command sets up a basic project structure that follows best practices and includes configuration templates.

## Arguments

- `PROJECT_NAME` (required): Name of the project to create. This will be used as the directory name.

## Options

- `-t, --template TEXT`: Project template to use. Available templates:
    - `basic` (default): Minimal project structure
    - `web`: Web application template
    - `api`: REST API template
    - `cli`: Command-line tool template
- `-d, --directory PATH`: Parent directory where project will be created (default: current directory)
- `--no-git`: Skip Git repository initialization
- `--no-venv`: Skip virtual environment creation
- `-f, --force`: Overwrite existing directory if it exists
- `--dry-run`: Show what would be created without actually creating anything
- `-v, --verbose`: Show detailed output

## Examples

### Basic Usage

Create a new project with default settings:

```bash
cli-tool init my-project
```

Output:
```
Creating project: my-project
✓ Created directory structure
✓ Initialized Git repository
✓ Created virtual environment
✓ Generated configuration files

Project 'my-project' created successfully!

Next steps:
  cd my-project
  source venv/bin/activate  # On Windows: venv\Scripts\activate
  cli-tool status
```

### Using a Template

Create a web application project:

```bash
cli-tool init my-web-app --template web
```

This creates a structure optimized for web applications:

```
my-web-app/
├── config/
│   ├── development.yaml
│   ├── production.yaml
│   └── staging.yaml
├── src/
│   ├── static/
│   │   ├── css/
│   │   └── js/
│   ├── templates/
│   └── app.py
├── tests/
│   └── test_app.py
├── .gitignore
├── requirements.txt
└── README.md
```

### Specify Custom Directory

Create project in a specific location:

```bash
cli-tool init my-project --directory ~/projects
```

### Skip Git Initialization

Create project without Git:

```bash
cli-tool init my-project --no-git
```

### Force Overwrite

Overwrite an existing directory:

```bash
cli-tool init my-project --force
```

**Warning**: This will delete the existing directory and all its contents!

### Dry Run

Preview what would be created:

```bash
cli-tool init my-project --dry-run
```

Output:
```
Dry run - no changes will be made
Would create: /path/to/my-project/
Would create: /path/to/my-project/config/
Would create: /path/to/my-project/src/
Would create: /path/to/my-project/tests/
Would create: /path/to/my-project/config/settings.yaml
Would create: /path/to/my-project/src/main.py
Would create: /path/to/my-project/README.md
Would initialize: Git repository
Would create: Python virtual environment
```

## Project Structure

### Default Structure

The default template creates:

```
project-name/
├── config/
│   └── settings.yaml      # Configuration file
├── src/
│   └── main.py           # Main application file
├── tests/
│   └── __init__.py       # Test package initializer
├── .gitignore            # Git ignore rules
├── requirements.txt      # Python dependencies
└── README.md            # Project documentation
```

### Configuration File

The generated `config/settings.yaml` includes:

```yaml
# Project configuration
project:
  name: project-name
  version: 0.1.0
  description: A new CLI tool project

# Environment settings
environment: development

# Logging configuration
logging:
  level: INFO
  format: "%(asctime)s - %(name)s - %(levelname)s - %(message)s"

# Additional settings can be added here
```

## Templates

### Available Templates

#### Basic Template
Minimal structure for simple projects:
- Basic directory layout
- Simple configuration
- Starter README

#### Web Template
For web applications:
- Static files directories (CSS, JS)
- Templates directory
- Web framework setup
- Development server configuration

#### API Template
For REST APIs:
- Route handlers structure
- API documentation setup
- Request/response models
- Authentication scaffolding

#### CLI Template
For command-line tools:
- Command structure
- Argument parser setup
- Help text templates
- Testing framework for CLI

## Exit Codes

- `0`: Success - project created successfully
- `1`: Error - invalid arguments or options
- `2`: Error - project directory already exists (without `--force`)
- `3`: Error - insufficient permissions
- `4`: Error - template not found

## Notes

- The command automatically detects if you're in a Git repository and adjusts accordingly
- Virtual environments are created using Python's built-in `venv` module
- All templates include a `.gitignore` file with common Python exclusions
- The generated README.md includes project-specific instructions

## Common Issues

### Directory Already Exists

**Problem**: Error message "Directory already exists"

**Solution**: Use `--force` to overwrite, or choose a different name:
```bash
cli-tool init my-project --force
```

### Permission Denied

**Problem**: Cannot create directory due to permissions

**Solution**: Either:
- Choose a directory where you have write permissions
- Use `sudo` (not recommended)
- Change the parent directory permissions

### Template Not Found

**Problem**: Invalid template name specified

**Solution**: Use `--template` with a valid template name (basic, web, api, or cli)

## See Also

- [deploy command](deploy.md): Deploy your initialized project
- [status command](status.md): Check project status
- [Getting Started Guide](../getting-started.md): Full setup instructions
