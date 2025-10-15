# Creating Releases

This guide explains how to create releases for the Kotlin LSP project.

## Quick Start

### Option 1: Manual Release via GitHub UI (推荐/Recommended)

1. Go to **Actions** tab in GitHub
2. Select **"Create Release"** workflow
3. Click **"Run workflow"**
4. Fill in the details:
   - **Branch**: Select the branch (usually `main`)
   - **Version**: Enter the version number (e.g., `0.0.1`)
   - **Mark as pre-release**: Check if this is a pre-release version
5. Click **"Run workflow"** button

The workflow will automatically:
- Build the VSIX extension
- Create a GitHub Release with the version tag
- Upload the VSIX file as a release asset

### Option 2: Automatic Release via Git Tags

1. Create and push a version tag:
   ```bash
   git tag v0.0.1
   git push origin v0.0.1
   ```

2. The workflow will automatically:
   - Detect the tag push
   - Build the VSIX extension
   - Create a GitHub Release

## Build Artifacts Without Release

If you just want to build the VSIX without creating a release:

1. Go to **Actions** tab in GitHub
2. Select **"Build VSIX"** workflow
3. Click **"Run workflow"**
4. Optionally check **"Push artifacts to release branch"** to push builds to `release` branch
5. Download the artifact from the workflow run

## Pre-release vs. Stable Release

**Pre-release** is marked when:
- Manual trigger: You check "Mark as pre-release"
- Tag trigger: Version contains keywords like `alpha`, `beta`, `rc`, or `preview`

Examples:
- `v0.0.1-alpha` → Pre-release
- `v0.1.0-beta.2` → Pre-release
- `v1.0.0-rc1` → Pre-release
- `v1.0.0` → Stable release

## Automated Daily Builds

The **Build VSIX** workflow runs automatically every day at 00:00 Beijing Time (16:00 UTC) and pushes artifacts to the `release` branch for testing purposes.

## Workflow Files

- **`.github/workflows/release.yml`** - Creates GitHub Releases
- **`.github/workflows/build-vsix.yml`** - Builds VSIX for testing/CI

For detailed workflow documentation, see [`.github/workflows/README.md`](.github/workflows/README.md).

## Troubleshooting

### Release not appearing

Make sure:
1. The workflow completed successfully (check Actions tab)
2. You have the correct permissions (`contents: write`)
3. The tag or version is correctly formatted

### VSIX file is missing

Check the workflow logs to see if:
1. The build step completed successfully
2. The package step generated the VSIX file
3. The upload step succeeded

---

## 中文说明

### 如何创建发布版本？

**方法一：通过 GitHub 界面手动触发（推荐）**

1. 打开 GitHub 仓库的 **Actions** 标签页
2. 选择 **"Create Release"** 工作流
3. 点击 **"Run workflow"** 按钮
4. 填写相关信息：
   - **分支**: 选择要发布的分支（通常是 `main`）
   - **版本号**: 输入版本号（例如：`0.0.1`）
   - **标记为预发布版**: 如果是预发布版本请勾选
5. 点击 **"Run workflow"** 开始构建

工作流会自动：
- 构建 VSIX 扩展文件
- 创建 GitHub Release 并打上版本标签
- 将 VSIX 文件上传为发布资源

**方法二：通过 Git 标签自动触发**

1. 创建并推送版本标签：
   ```bash
   git tag v0.0.1
   git push origin v0.0.1
   ```

2. 工作流会自动运行并创建发布版本

### 仅构建不发布

如果只想构建 VSIX 文件而不创建正式发布：

1. 打开 **Actions** 标签页
2. 选择 **"Build VSIX"** 工作流
3. 点击 **"Run workflow"**
4. 可选择勾选 **"Push artifacts to release branch"** 将构建推送到 `release` 分支
5. 从工作流运行记录中下载生成的文件
