# package-sample

A collection of shadcn/ui components packaged for reuse.

## Installation

```bash
npm install package-sample
```

## Usage

Import components directly:

```tsx
import { Button } from "package-sample"

export default function App() {
  return <Button>Click me</Button>
}
```

## Configuration

### Tailwind CSS

Add the package paths to your `tailwind.config.js`:

```js
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: [
    "./src/**/*.{ts,tsx}",
    "./node_modules/package-sample/dist/**/*.{js,mjs}", // Add this line
  ],
  // ...
}
```

### Styles

Import the CSS file in your root layout or entry point:

```tsx
import "package-sample/index.css"
```

## Publishing (for maintainers)

This package uses GitHub Actions for automated publishing with npm Trusted Publishers (OIDC).

### Setup (One-time)

1. **Configure Trusted Publishers on npmjs.com**:
   - Go to https://www.npmjs.com/package/package-sample-test/access
   - Click "Publishing access" → "Add trusted publisher"
   - Select "GitHub Actions"
   - Enter repository details and workflow name: `publish.yml`

2. **Push your repository to GitHub**

### Publishing a New Version

```bash
# Update version and create a git tag
npm run release patch  # 1.0.0 → 1.0.1
# or
npm run release minor  # 1.0.0 → 1.1.0
# or
npm run release major  # 1.0.0 → 2.0.0

# Push the tag to trigger publishing
git push --follow-tags
```

GitHub Actions will automatically:
- Build the package
- Run tests
- Publish to npm with provenance attestation

### CI/CD

- **Pull Requests**: Automatically build and test
- **Tags (v*)**: Automatically publish to npm
