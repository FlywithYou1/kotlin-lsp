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
- Scheduled daily at 00:00 Beijing Time (16:00 UTC)

### What it does

1. Checks out the repository
2. Sets up Node.js environment (version 22)
3. Installs npm dependencies
4. Compiles the TypeScript extension code
5. Creates a dummy LSP server archive (for testing/development purposes)
6. Packages the extension into a VSIX file using `vsce`
7. Uploads the VSIX file as a workflow artifact (retained for 30 days)
8. **Creates GitHub Release** (for scheduled builds and pushes to `main` branch)
9. (Scheduled runs or manual with option) Pushes build artifacts to the `release` branch

### Output

The workflow generates a VSIX file named `kotlin-vscode-<version>.vsix` which can be:
- Downloaded from the GitHub Actions artifacts
- **Downloaded from GitHub Releases** (for scheduled builds and main branch pushes)
- Installed in VS Code via `Extensions > Install from VSIX...`

### Automated Releases

When triggered by:
- **Scheduled runs** (daily at 00:00 Beijing Time)
- **Pushes to `main` branch**

The workflow automatically creates a GitHub Release with:
- Pre-release tag (e.g., `v0.0.1-build.20251015-180000`)
- VSIX file attached as a downloadable asset
- Build metadata (timestamp, commit SHA)
- Marked as pre-release (not "latest")

### Notes

- This workflow creates a lightweight VSIX with a dummy LSP server for testing purposes
- For production builds with the full LSP server, use the build process described in the main README
- The VSIX artifact is retained for 30 days after the workflow run
- **Automated builds create GitHub Releases** for easy access to latest builds
- When triggered by the daily schedule, build artifacts are also pushed to the `release` branch
- The scheduled build runs daily at 00:00 Beijing Time (16:00 UTC)

### Manual Trigger

You can manually trigger this workflow from the GitHub Actions tab:
1. Go to Actions > Build VSIX
2. Click "Run workflow"
3. Select the branch
4. Optionally check "Push artifacts to release branch" to push to release branch
5. Click "Run workflow" button

## Create Release Workflow

**File:** `release.yml`

**Purpose:** Creates GitHub Releases with VSIX artifacts for distribution.

### Triggers

The workflow runs on:
- Push of version tags (e.g., `v0.0.1`, `v1.2.3`)
- Manual dispatch via GitHub Actions UI

### What it does

1. Checks out the repository
2. Sets up Node.js environment (version 22)
3. Installs npm dependencies
4. Compiles the TypeScript extension code
5. Creates a dummy LSP server archive
6. Determines the version from tag or manual input
7. Updates package.json with the specified version
8. Packages the extension into a VSIX file
9. Creates a GitHub Release with the VSIX file attached

### Output

Creates a GitHub Release with:
- Release tag (e.g., `v0.0.1`)
- Release title and description
- VSIX file as a downloadable asset
- Automatic pre-release detection based on version or manual input

### Manual Trigger

You can manually trigger this workflow from the GitHub Actions tab:
1. Go to Actions > Create Release
2. Click "Run workflow"
3. Enter the version (e.g., `0.0.1`)
4. Optionally check "Mark as pre-release"
5. Click "Run workflow" button

### Automatic Trigger via Tags

To automatically create a release:
1. Create and push a version tag:
   ```bash
   git tag v0.0.1
   git push origin v0.0.1
   ```
2. The workflow will automatically run and create a GitHub Release

### Pre-release Detection

Releases are marked as pre-release if:
- Manual trigger: The "Mark as pre-release" option is checked
- Tag trigger: The version contains keywords like `alpha`, `beta`, `rc`, or `preview`

## Important Notes

- Both workflows require `contents: write` permission to create releases and push to branches
- The workflows use a dummy LSP server for packaging - full production builds should include the actual LSP server
- VSIX files from workflow artifacts expire after 30 days, but GitHub Releases persist indefinitely
