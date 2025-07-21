# Cross Chain SDK - Local Development Guide

This guide is for team members who need to use a locally developed version of the Cross Chain SDK in other projects.

## Prerequisites

- Node.js (version 18.16.0 or higher)
- pnpm package manager

## Setting Up the SDK for Local Development

### 1. Clone and Link the Required Fusion SDK

Before building this SDK, you must clone and link the required [fusion-sdk-1inch](https://github.com/malawadd/fusion-sdk-1inch) package:

```bash
# Clone the fusion-sdk-1inch repository (if you haven't already)
git clone https://github.com/malawadd/fusion-sdk-1inch.git
cd fusion-sdk-1inch

# Install dependencies and build
pnpm install
pnpm build
pnpm link --global
```

### 2. Clone and Setup the Cross Chain SDK Repository

```bash
# Go to the cross-chain-sdk directory

git clone  https://github.com/malawadd/1inch-cross
cd 1inch-cross

# Install dependencies
pnpm install

# Link the package globally
pnpm link --global @1inch/fusion-sdk

# Build the SDK
pnpm build
```

### 3. Create a Global Link for the Cross Chain SDK

```bash
# Create a global symlink for the SDK
pnpm link --global
```

This creates a global symlink that other projects can reference.

## Using the Linked SDK in Your Project

### 1. Remove the Published Version (If Already Installed)

If your project already has `@1inch/cross-chain-sdk` in its `package.json`, you need to remove it first:

```json
// In your package.json, remove this line from dependencies:
"@1inch/cross-chain-sdk": "v0.1.15-rc.0",  // Remove this entire line
```

Then clean the installed packages:

```bash
cd /path/to/your/project
pnpm install
```

### 2. Link the SDK to Your Project

Navigate to your project directory and link the SDK:

```bash
cd /path/to/your/project
pnpm link --global @1inch/cross-chain-sdk
```

### 3. Install Dependencies

Reinstall dependencies to ensure everything works with the linked version:

```bash
pnpm install
```

### 4. Use the SDK in Your Code

You can now import and use the SDK as normal:

```typescript
import { SDK, NetworkEnum, HashLock } from '@1inch/cross-chain-sdk'

const sdk = new SDK({
    url: 'https://api.1inch.dev/fusion-plus',
    authKey: 'your-auth-key'
})
```

## Making Changes to the SDK

When you make changes to the SDK code:

### 1. Rebuild the SDK

```bash
cd /path/to/cross-chain-sdk
pnpm build
```

### 2. Restart Your Development Server

The changes should be automatically picked up by your linked project. If not, restart your development server.

## Development Commands

While working on the SDK, you can use these commands:

```bash
# Run tests
pnpm test

# Run linter
pnpm lint

# Fix linting issues
pnpm lint --fix

# Type checking
pnpm lint:types

# Run all quality checks
pnpm qa:fix
```

## Unlinking the SDK

When you're done with local development and want to switch back to the published version:

### 1. Unlink from Your Project

```bash
cd /path/to/your/project
pnpm unlink @1inch/cross-chain-sdk
```

### 2. Add the Published Version Back to package.json

Add the SDK back to your `package.json` dependencies:

```json
"dependencies": {
  "@1inch/cross-chain-sdk": "v0.1.15-rc.0"
}
```

### 3. Install the Published Version

```bash
pnpm install
```

### 4. Remove Global Link (Optional)

```bash
cd /path/to/cross-chain-sdk
pnpm unlink --global
```

## Troubleshooting

### Link Not Working

If the link isn't working properly:

1. Make sure you've built the SDK (`pnpm build`)
2. Check that the global link exists: `pnpm list --global --depth=0`
3. Try unlinking and relinking both globally and in your project

### TypeScript Issues

If you're experiencing TypeScript issues:

1. Make sure the SDK is built (`pnpm build`)
2. Restart your TypeScript language server
3. Check that your project's TypeScript version is compatible

### Dependency Conflicts

If you encounter dependency conflicts:

1. Check that peer dependencies are properly installed
2. Consider using `pnpm install --shamefully-hoist` in your project
3. Ensure Node.js versions match between SDK and your project

## Notes

- Always rebuild the SDK after making changes (`pnpm build`)
- The linked version will override any installed version of `@1inch/cross-chain-sdk`
- Remember to unlink when switching back to the published version
- Changes to the SDK's `package.json` may require relinking