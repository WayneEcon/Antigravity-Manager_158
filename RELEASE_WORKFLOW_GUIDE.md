# GitHub Actions Release Workflow Guide

## Overview

This repository is configured with a comprehensive GitHub Actions workflow that automatically builds Tauri desktop applications for multiple platforms and creates releases.

## Workflow Features

The `.github/workflows/release.yml` workflow supports:

### ✅ Multi-Platform Builds
- **macOS**:
  - aarch64 (Apple Silicon M1/M2/M3)
  - x86_64 (Intel Macs)
  - Universal binary (works on both architectures)
- **Windows**: x86_64 (Windows 10/11)
- **Linux**: 
  - Ubuntu 22.04 (x86_64)
  - Ubuntu 24.04 ARM64

### ✅ Build Artifacts
The workflow generates platform-specific installation packages:
- **macOS**: `.dmg` files and `.app.tar.gz` archives
- **Windows**: `.msi` and `.exe` installers
- **Linux**: `.AppImage`, `.deb` packages

### ✅ Automatic Release Creation
- Creates draft releases with all build artifacts
- Uploads updater JSON for auto-update functionality
- Includes release notes

## How to Trigger the Workflow

There are two ways to trigger the release workflow:

### Method 1: Manual Trigger (Recommended for Testing)

1. Go to your GitHub repository
2. Click on **Actions** tab
3. Select **Release** workflow from the left sidebar
4. Click **Run workflow** button (on the right side)
5. Select the branch you want to build from
6. Click the green **Run workflow** button

**Steps in detail:**
```
GitHub Repository → Actions → Release → Run workflow → Select branch → Run workflow
```

### Method 2: Tag-Based Automatic Trigger

Push a version tag to automatically trigger the release:

```bash
# Create and push a version tag
git tag v1.15.8
git push origin v1.15.8
```

The workflow will automatically:
1. Build for all platforms
2. Create a draft release named "Antigravity Tools v1.15.8"
3. Upload all installation packages as release assets

## After the Workflow Completes

1. Go to the **Releases** page in your repository
2. Find the draft release (it won't be published automatically)
3. Review the release and the uploaded artifacts
4. Edit the release notes if needed
5. Click **Publish release** to make it public

## Expected Build Artifacts

After a successful build, you should see the following files in the release:

- `Antigravity-Tools_1.15.8_aarch64.dmg` - macOS Apple Silicon installer
- `Antigravity-Tools_1.15.8_x64.dmg` - macOS Intel installer
- `Antigravity-Tools_1.15.8_universal.dmg` - macOS Universal installer
- `Antigravity-Tools_1.15.8_x64_en-US.msi` - Windows installer (MSI)
- `Antigravity-Tools_1.15.8_x64-setup.exe` - Windows installer (EXE)
- `antigravity-tools_1.15.8_amd64.AppImage` - Linux AppImage
- `antigravity-tools_1.15.8_amd64.deb` - Debian/Ubuntu package
- `antigravity-tools_1.15.8_arm64.AppImage` - Linux ARM AppImage
- `antigravity-tools_1.15.8_arm64.deb` - Debian/Ubuntu ARM package
- `updater.json` - Auto-updater configuration

## Troubleshooting

### Build Failures

If the workflow fails:

1. Check the **Actions** tab for error logs
2. Click on the failed workflow run
3. Examine the logs for each job to identify the issue

Common issues:
- **Dependency installation fails**: Check if all required system dependencies are available
- **Rust compilation errors**: Ensure the Rust code compiles locally first
- **TypeScript errors**: Run `npm run build` locally to verify
- **Signing errors**: Ensure `TAURI_SIGNING_PRIVATE_KEY` and `TAURI_SIGNING_PRIVATE_KEY_PASSWORD` secrets are set (optional for draft releases)

### Version Numbers

The workflow uses the tag name or can be manually triggered. The version in `src-tauri/tauri.conf.json` should match your desired release version.

Current version in config: **4.0.7**

If you want to release version 1.15.8, update the version in:
- `src-tauri/tauri.conf.json`
- `package.json`

Then commit and either create a tag `v1.15.8` or manually trigger the workflow.

## Security Notes

The workflow:
- Runs Rust tests before building (`cargo test --all --all-features`)
- Uses trusted GitHub Actions (from verified publishers)
- Creates draft releases by default (requires manual review before publishing)
- Supports code signing via `TAURI_SIGNING_PRIVATE_KEY` (optional)

## Next Steps

1. ✅ The TypeScript type error has been fixed
2. ✅ The workflow is configured and ready to use
3. 🎯 Update version numbers if needed (currently 4.0.7 in config)
4. 🎯 Trigger the workflow using one of the methods above
5. 🎯 Download and test the generated installers
6. 🎯 Publish the release when ready

---

**Note**: The workflow creates **draft releases** by default. This gives you a chance to review everything before making the release public.
