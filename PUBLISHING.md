# Publishing Your BMAD-METHOD Fork to npm

This guide explains how to publish your fork to npm so users can install it with a single command.

## Prerequisites

1. **npm Account**: Create a free account at [npmjs.com](https://www.npmjs.com/signup)
2. **npm CLI**: Already installed with Node.js
3. **Repository Access**: Push access to your fork

## One-Time Setup

### 1. Login to npm

```bash
npm login
```

Enter your npm credentials when prompted.

### 2. Verify Your Login

```bash
npm whoami
```

Should display: `jamon8888`

## Publishing Process

### Option A: Publish Stable Version

```bash
# Make sure all changes are committed
git add .
git commit -m "chore: prepare for npm publish"

# Publish to npm (public)
npm publish --access public

# Push to GitHub
git push origin claude/fix-local-install-01Kfj3y9QuU8naqyMrcHzj1K
```

### Option B: Publish Alpha/Beta Version

```bash
# Tag as alpha version
npm publish --access public --tag alpha

# Or tag as beta
npm publish --access public --tag beta
```

## After Publishing

Users can now install your fork with:

```bash
# Latest stable version
npx @jamon8888/bmad-method install

# Specific version tag
npx @jamon8888/bmad-method@alpha install
```

## Updating Your Package

When you make changes and want to publish updates:

```bash
# Update version (choose one)
npm version patch  # 6.0.0-alpha.12 -> 6.0.0-alpha.13
npm version minor  # 6.0.0-alpha.12 -> 6.1.0-alpha.0
npm version major  # 6.0.0-alpha.12 -> 7.0.0-alpha.0

# Publish the update
npm publish --access public

# Push version tag to GitHub
git push --follow-tags
```

## Version Management

Your package is currently at: **6.0.0-alpha.12**

**Versioning Strategy:**

- **Patch** (x.x.X): Bug fixes, small improvements
- **Minor** (x.X.0): New features, non-breaking changes
- **Major** (X.0.0): Breaking changes

**Version Tags:**

- `latest`: Stable release (default for `npx @jamon8888/bmad-method`)
- `alpha`: Alpha releases (install with `npx @jamon8888/bmad-method@alpha`)
- `beta`: Beta releases (install with `npx @jamon8888/bmad-method@beta`)

## Package Information

- **Package Name**: `@jamon8888/bmad-method`
- **Scope**: `@jamon8888` (your npm username)
- **Repository**: https://github.com/jamon8888/BMAD-METHOD
- **Current Version**: 6.0.0-alpha.12

## Troubleshooting

### Error: 403 Forbidden

You don't have permission. Make sure:

1. You're logged in: `npm whoami`
2. Package is scoped to your username: `@jamon8888/...`
3. You're using `--access public` flag

### Error: Package already exists

The name is taken. Your scoped package `@jamon8888/bmad-method` should be available.

### Error: Version already published

You can't republish the same version. Bump the version:

```bash
npm version patch
npm publish --access public
```

## Alternative: GitHub-Only Installation

If you prefer not to publish to npm, users can still install directly from GitHub:

```bash
# From default branch
npx github:jamon8888/BMAD-METHOD install

# From specific branch
npx github:jamon8888/BMAD-METHOD#claude/fix-local-install-01Kfj3y9QuU8naqyMrcHzj1K install

# From specific tag/release
npx github:jamon8888/BMAD-METHOD#v6.0.0 install
```

## Best Practices

1. **Test Before Publishing**: Always run `npm pack --dry-run` to verify package contents
2. **Semantic Versioning**: Follow semver principles (major.minor.patch)
3. **Changelog**: Update CHANGELOG.md with each release
4. **Git Tags**: Push version tags to GitHub for release tracking
5. **Alpha/Beta Tags**: Use distribution tags for pre-release versions

## Automation (Optional)

Consider setting up GitHub Actions to automatically publish on releases. See `.github/workflows/manual-release.yaml` for reference.

## Questions?

- npm documentation: https://docs.npmjs.com/packages-and-modules/contributing-packages-to-the-registry
- Scoped packages: https://docs.npmjs.com/creating-and-publishing-scoped-public-packages
