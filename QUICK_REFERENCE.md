# Quick Reference - Angular Boilerplate Commands

## Development
\`\`\`bash
# Start dev server
npm start
nx serve main-app

# Build
npm run build
nx build main-app --configuration production
\`\`\`

## Testing
\`\`\`bash
# Run tests
npm test
nx test main-app

# With coverage
npm test -- --code-coverage
\`\`\`

## Code Quality
\`\`\`bash
# Lint code
npm run lint
nx lint main-app

# Format code
npm run format
npx prettier --write "**/*.{ts,html,scss,json}"
\`\`\`

## Nx Commands
\`\`\`bash
# View dependency graph
npm run dep-graph

# Check affected projects
nx affected:apps --base=main
nx affected:libs --base=main

# Build affected
nx affected:build --base=main
\`\`\`

## Generate
\`\`\`bash
# New app
nx generate @nrwl/angular:app my-app

# New library
nx generate @nrwl/angular:lib my-lib --directory=libs/shared

# New component
nx generate @nrwl/angular:component my-component --project=main-app
\`\`\`

## Documentation
- See README.md for overview
- See SETUP_GUIDE.md for installation
- See DEVELOPMENT.md for workflows
- See TESTING.md for testing guides
