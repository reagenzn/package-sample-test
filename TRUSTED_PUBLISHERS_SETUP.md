# npm Trusted Publishers Setup Guide

## What is Trusted Publishers?

Trusted Publishers (OIDC) allows GitHub Actions to publish npm packages without using long-lived tokens. This is the recommended approach as of npm's 2024 security updates.

## Setup Steps

### 1. Publish Your Package Manually First (One-time)

If this is the first time publishing the package, you need to publish it manually once:

```bash
# Make sure you're logged in
npm whoami

# Publish manually
npm publish --access public
```

### 2. Configure Trusted Publishers on npmjs.com

1. Go to your package settings:
   - URL: `https://www.npmjs.com/package/package-sample-test/access`
   - Or: npmjs.com → Your profile → Packages → package-sample-test → Settings

2. Navigate to "Publishing access" section

3. Click "Add trusted publisher"

4. Select **"GitHub Actions"**

5. Fill in the form:
   - **Provider**: GitHub Actions (already selected)
   - **Repository owner**: Your GitHub username (e.g., `reagenzn`)
   - **Repository name**: Repository name (e.g., `package-sample`)
   - **Workflow filename**: `publish.yml`
   - **Environment name**: (leave empty, or specify if using GitHub Environments)

6. Click "Add"

### 3. Verify Configuration

After adding the trusted publisher, you should see:
- Provider: GitHub
- Repository: `your-username/package-sample`
- Workflow: `publish.yml`

### 4. Push Your Code to GitHub

```bash
# Initialize git if not already done
git init
git add .
git commit -m "Initial commit with CI/CD"

# Add remote and push
git remote add origin https://github.com/your-username/package-sample.git
git branch -M main
git push -u origin main
```

### 5. Test the Publishing Workflow

```bash
# Create a new version and tag
npm version patch  # or minor/major

# Push the tag to trigger the workflow
git push --follow-tags
```

## Troubleshooting

### "Unable to authenticate" error

- Make sure you've configured Trusted Publishers correctly on npmjs.com
- Verify the repository name and workflow filename match exactly
- Check that the GitHub Actions workflow has `id-token: write` permission

### "Package not found" error

- You must publish the package manually at least once before using Trusted Publishers
- Run `npm publish --access public` manually first

### "Provenance failed" error

- Ensure you're using `npm publish --provenance` in the workflow
- Check that Node.js version is 16 or higher in the workflow

## Additional Notes

- **No npm token needed**: The workflow uses OIDC, no `NPM_TOKEN` secret required
- **Automatic provenance**: Every publish includes cryptographic proof of origin
- **Security**: Short-lived credentials (valid only during workflow execution)
- **Audit trail**: Full transparency on where packages were built

## Reference

- [npm Trusted Publishers Documentation](https://docs.npmjs.com/trusted-publishers)
- [npm Security Updates](https://github.blog/changelog/2025-09-29-strengthening-npm-security-important-changes-to-authentication-and-token-management/)
