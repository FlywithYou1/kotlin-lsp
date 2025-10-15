# GitHub Actions Workflows

This directory contains GitHub Actions workflows for the Kotlin LSP project.

## Build VSIX Workflow

**File:** `build-vsix.yml`

**Purpose:** Automatically builds the VSCode extension (VSIX file) for the Kotlin Language Server.

### Triggers

The workflow runs on:
- Push to `main` branch
- Push to branches matching `copilot/**`
- Pull requests targeting `main` branch
- Manual dispatch via GitHub Actions UI

### What it does

1. Checks out the repository
2. Sets up Node.js environment (version 22)
3. Installs npm dependencies
4. Compiles the TypeScript extension code
5. Creates a dummy LSP server archive (for testing/development purposes)
6. Packages the extension into a VSIX file using `vsce`
7. Uploads the VSIX file as a workflow artifact (retained for 30 days)

### Output

The workflow generates a VSIX file named `kotlin-vscode-<version>.vsix` which can be:
- Downloaded from the GitHub Actions artifacts
- Installed in VS Code via `Extensions > Install from VSIX...`

### Notes

- This workflow creates a lightweight VSIX with a dummy LSP server for testing purposes
- For production builds with the full LSP server, use the build process described in the main README
- The VSIX artifact is retained for 30 days after the workflow run

### Manual Trigger

You can manually trigger this workflow from the GitHub Actions tab:
1. Go to Actions > Build VSIX
2. Click "Run workflow"
3. Select the branch
4. Click "Run workflow" button
