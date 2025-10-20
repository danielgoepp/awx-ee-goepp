# Claude Development Notes - AWX EE Goepp

## Project Context

This project is for building a custom Ansible Execution Environment (EE) for AWX/Ansible Automation Platform. The execution environment is a containerized runtime that includes all dependencies needed to run Ansible automation.

## Development Environment Setup

- Python virtual environment located at `.venv/`
- ansible-builder 3.1.0 installed
- VSCode configured to use .venv as default interpreter
- Terminal auto-activation enabled for the virtual environment

## Git Repository

- Repository initialized with `main` as default branch
- `.claude/` directory excluded from version control
- Build artifacts (`context/`) gitignored
- Virtual environments and IDE files gitignored

## Key Components

### execution-environment.yml
Main configuration file that defines everything inline:
- Base container image (CentOS Stream 9)
- Python dependencies (inline in `python:` section)
- Ansible collections (inline in `galaxy:` section)
- System packages (inline in `system:` section)
- Additional build steps (Helm installation, Receptor integration, Git LFS setup)

All dependencies are defined directly in this single file rather than separate requirements files.

## Build Process

The `ansible-builder` tool:
1. Reads the execution-environment.yml configuration
2. Generates a multi-stage Containerfile/Dockerfile
3. Builds the container image with all dependencies
4. Creates a final execution environment image

## Common Commands

```bash
# Build the EE
ansible-builder build --tag awx-ee-goepp:latest

# Build with verbose output
ansible-builder build --tag awx-ee-goepp:latest -v 3

# Create only the build context (no build)
ansible-builder create

# Use podman instead of docker
ansible-builder build --container-runtime podman --tag awx-ee-goepp:latest
```

## Notes

- The EE is based on CentOS Stream 9 with Python 3.11
- All dependencies are defined inline in execution-environment.yml
- Build artifacts are stored in the `context/` directory (gitignored)
- The final image can be pushed to a container registry for use in AWX
- Git repository initialized with proper gitignore configuration
