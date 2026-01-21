# Frequently Asked Questions (FAQ)

This page addresses common questions, issues, and troubleshooting tips for using the CLI tool.

## General Questions

### What is the CLI tool?

The CLI tool is a command-line interface utility designed to streamline common development and deployment workflows. It provides simple commands for initializing projects, managing deployments, and monitoring application status.

### What are the system requirements?

- **Operating System**: Windows, macOS, or Linux
- **Python**: Version 3.8 or higher
- **Disk Space**: Minimum 100MB for installation and dependencies
- **Network**: Internet connection for initial setup and cloud deployments

### Is the CLI tool free?

Yes, the CLI tool is open-source and released under the MIT License. You can use it freely for personal and commercial projects.

### How do I update to the latest version?

```bash
pip install --upgrade cli-tool
```

Check your current version:
```bash
cli-tool --version
```

## Installation Issues

### "Command not found: cli-tool"

**Problem**: After installation, the command is not recognized.

**Solutions**:

1. **Check if Python's bin directory is in PATH**:
   ```bash
   # On Linux/macOS
   echo $PATH | grep python
   
   # Add to PATH if needed (add to ~/.bashrc or ~/.zshrc)
   export PATH="$HOME/.local/bin:$PATH"
   ```

2. **Use Python module execution**:
   ```bash
   python -m cli_tool --version
   ```

3. **Install with user flag**:
   ```bash
   pip install --user cli-tool
   ```

### "Permission denied" during installation

**Problem**: Permission error when running pip install.

**Solutions**:

1. **Install for current user only**:
   ```bash
   pip install --user cli-tool
   ```

2. **Use a virtual environment** (recommended):
   ```bash
   python -m venv myenv
   source myenv/bin/activate  # On Windows: myenv\Scripts\activate
   pip install cli-tool
   ```

### Dependencies fail to install

**Problem**: Error installing required packages.

**Solutions**:

1. **Upgrade pip**:
   ```bash
   pip install --upgrade pip
   ```

2. **Install build tools**:
   ```bash
   # On Ubuntu/Debian
   sudo apt-get install python3-dev build-essential
   
   # On macOS
   xcode-select --install
   ```

3. **Check Python version**:
   ```bash
   python --version
   # Should be 3.8 or higher
   ```

## Configuration Issues

### Where is the configuration file stored?

The configuration file is located at:
- **Linux/macOS**: `~/.cli-tool/config.yaml`
- **Windows**: `C:\Users\<username>\.cli-tool\config.yaml`

### How do I reset my configuration?

```bash
# Reset to defaults
cli-tool config reset

# Or delete the configuration file manually
rm ~/.cli-tool/config.yaml  # Linux/macOS
del %USERPROFILE%\.cli-tool\config.yaml  # Windows
```

### Configuration changes aren't taking effect

**Problem**: Changes to config.yaml don't seem to work.

**Solutions**:

1. **Validate YAML syntax**:
   ```bash
   cli-tool config validate
   ```

2. **Use the config command**:
   ```bash
   cli-tool config set key value
   ```

3. **Check for multiple config files**:
   ```bash
   cli-tool config path
   ```

### "Invalid configuration" error

**Problem**: Configuration file has syntax errors.

**Solution**:

1. Check YAML formatting (indentation matters!)
2. Validate configuration:
   ```bash
   cli-tool config validate --verbose
   ```
3. Restore from backup or reset to defaults

## Project Initialization Issues

### "Directory already exists" error

**Problem**: Cannot initialize project because directory exists.

**Solutions**:

1. **Use a different name**:
   ```bash
   cli-tool init my-project-v2
   ```

2. **Force overwrite** (WARNING: deletes existing content):
   ```bash
   cli-tool init my-project --force
   ```

3. **Remove existing directory first**:
   ```bash
   rm -rf my-project
   cli-tool init my-project
   ```

### Template not found

**Problem**: "Template 'xyz' not found" error.

**Solution**: Use a valid template name:
```bash
cli-tool init my-project --template web
# Valid templates: basic, web, api, cli
```

### Git initialization fails

**Problem**: "Failed to initialize Git repository".

**Solutions**:

1. **Install Git**:
   ```bash
   # Check if Git is installed
   git --version
   
   # Install Git if needed
   # On Ubuntu/Debian: sudo apt-get install git
   # On macOS: brew install git
   ```

2. **Skip Git initialization**:
   ```bash
   cli-tool init my-project --no-git
   ```

## Deployment Issues

### "Deployment failed" with no clear reason

**Problem**: Deployment fails without specific error message.

**Solutions**:

1. **Check detailed logs**:
   ```bash
   cli-tool deploy --env production --verbose
   ```

2. **Validate configuration**:
   ```bash
   cli-tool config validate
   ```

3. **Check environment status**:
   ```bash
   cli-tool status --env production
   ```

4. **Try dry run first**:
   ```bash
   cli-tool deploy --env production --dry-run
   ```

### "Authentication failed" during deployment

**Problem**: Cannot authenticate with cloud provider.

**Solutions**:

1. **Check credentials**:
   ```bash
   cli-tool config check
   ```

2. **Update credentials**:
   ```bash
   cli-tool config set cloud.access_key YOUR_ACCESS_KEY
   cli-tool config set cloud.secret_key YOUR_SECRET_KEY
   ```

3. **Use environment variables**:
   ```bash
   export AWS_ACCESS_KEY_ID=your_key
   export AWS_SECRET_ACCESS_KEY=your_secret
   ```

### Build fails before deployment

**Problem**: Build step fails, preventing deployment.

**Solutions**:

1. **Check build logs**:
   ```bash
   cli-tool build --verbose
   ```

2. **Verify dependencies**:
   ```bash
   pip install -r requirements.txt
   ```

3. **Skip build temporarily**:
   ```bash
   cli-tool deploy --no-build
   # (Not recommended for production)
   ```

### Health checks fail after deployment

**Problem**: Deployment completes but health checks timeout.

**Solutions**:

1. **Check application logs**:
   ```bash
   cli-tool logs --env production --tail 100
   ```

2. **Increase health check timeout**:
   ```yaml
   # In config/<env>.yaml
   health_check:
     timeout: 600  # Increase from default 300
   ```

3. **Verify health endpoint**:
   ```bash
   curl https://your-app.com/health
   ```

4. **Check for application errors**:
   ```bash
   cli-tool status --env production --detailed
   ```

### Rollback fails

**Problem**: Automatic or manual rollback doesn't work.

**Solutions**:

1. **Check deployment history**:
   ```bash
   cli-tool history --env production
   ```

2. **Specify exact version**:
   ```bash
   cli-tool rollback --env production --version 1.2.1
   ```

3. **Manual intervention may be needed** - contact your DevOps team

## Status and Monitoring Issues

### "Unable to connect to environment"

**Problem**: Status command cannot connect to environment.

**Solutions**:

1. **Verify network connectivity**:
   ```bash
   ping your-app.com
   ```

2. **Check credentials**:
   ```bash
   cli-tool config check
   ```

3. **Verify environment is running**:
   - Check cloud provider console
   - Check with your team

### Status shows "degraded" but application seems fine

**Problem**: Health status shows degraded despite app working.

**Investigation steps**:

1. **Check detailed status**:
   ```bash
   cli-tool status --detailed
   ```

2. **Review health checks**:
   ```bash
   cli-tool status --health-only
   ```

3. **Check resource usage**:
   ```bash
   cli-tool status --metrics
   ```

Common causes:
- Slow database queries
- High CPU/memory usage
- External API timeouts
- Cache connection issues

## Performance Issues

### Commands are slow to execute

**Problem**: CLI commands take too long to run.

**Solutions**:

1. **Check network latency**:
   ```bash
   ping your-cloud-provider.com
   ```

2. **Use local caching** (if available):
   ```bash
   cli-tool config set cache.enabled true
   ```

3. **Reduce verbosity**:
   ```bash
   cli-tool status --quiet
   ```

### Large projects take too long to initialize

**Problem**: `cli-tool init` is slow for large projects.

**Solutions**:

1. **Skip virtual environment creation**:
   ```bash
   cli-tool init my-project --no-venv
   ```

2. **Use minimal template**:
   ```bash
   cli-tool init my-project --template basic
   ```

## Common Error Messages

### "No such file or directory"

Usually means the CLI tool is looking for a file that doesn't exist.

**Check**:
- You're in the correct directory
- Required configuration files exist
- File paths in config are correct

### "Invalid YAML syntax"

Configuration file has formatting errors.

**Solution**: Validate and fix YAML:
```bash
cli-tool config validate --verbose
```

### "Operation not permitted"

Insufficient permissions for the operation.

**Solutions**:
- Check file/directory permissions
- Verify cloud provider IAM policies
- Ensure you're logged in with correct account

### "Connection timeout"

Network connection to service failed.

**Solutions**:
- Check internet connectivity
- Verify firewall settings
- Check if service is down
- Try again later

## Best Practices

### How often should I update the CLI tool?

Check for updates monthly or when:
- New features are announced
- Security updates are released
- You encounter bugs that might be fixed

### Should I use different configurations for each project?

Yes, recommended approach:

```bash
# Project-specific config
cd my-project
cli-tool config set --local project.name my-project
```

### What's the recommended deployment workflow?

```bash
# 1. Test locally
cli-tool build && cli-tool test

# 2. Deploy to staging
cli-tool deploy --env staging --test

# 3. Verify staging
cli-tool status --env staging

# 4. Deploy to production (with safety checks)
cli-tool deploy --env production --dry-run
cli-tool deploy --env production --test --rollback-on-failure

# 5. Monitor production
cli-tool status --env production --watch
```

### How do I debug issues?

1. **Enable verbose logging**:
   ```bash
   cli-tool --verbose <command>
   ```

2. **Check logs**:
   ```bash
   cli-tool logs --tail 100
   ```

3. **Validate configuration**:
   ```bash
   cli-tool config validate --verbose
   ```

4. **Check status**:
   ```bash
   cli-tool status --detailed
   ```

## Getting Help

### Where can I find more documentation?

- [Getting Started Guide](getting-started.md)
- [Command Reference](commands/init.md)
- [GitHub Repository](https://github.com/yourusername/cli-tool)
- [Issue Tracker](https://github.com/yourusername/cli-tool/issues)

### How do I report a bug?

1. Check if it's already reported in [GitHub Issues](https://github.com/yourusername/cli-tool/issues)
2. If not, create a new issue with:
   - CLI tool version (`cli-tool --version`)
   - Operating system
   - Complete error message
   - Steps to reproduce
   - Expected vs actual behavior

### How do I request a feature?

Submit a feature request on [GitHub Discussions](https://github.com/yourusername/cli-tool/discussions) with:
- Clear description of the feature
- Use case explaining why it's needed
- Examples of how it would work

### Where can I get community support?

- **GitHub Discussions**: For questions and discussions
- **Stack Overflow**: Tag questions with `cli-tool`
- **Discord/Slack**: Join our community chat (link in GitHub)

## Troubleshooting Checklist

When encountering issues, work through this checklist:

- [ ] Check CLI tool version: `cli-tool --version`
- [ ] Verify Python version: `python --version` (≥3.8)
- [ ] Validate configuration: `cli-tool config validate`
- [ ] Check connectivity: `cli-tool config check`
- [ ] Review logs: `cli-tool logs --tail 100`
- [ ] Try with verbose output: `cli-tool --verbose <command>`
- [ ] Check environment status: `cli-tool status --env <env>`
- [ ] Search existing issues on GitHub
- [ ] Try with fresh configuration: `cli-tool config reset`
- [ ] Reinstall if necessary: `pip install --force-reinstall cli-tool`

Still stuck? Reach out to the community or file an issue on GitHub!
