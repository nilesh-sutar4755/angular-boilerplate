# Development Workflows

This document outlines common development workflows and best practices for working with the Angular Boilerplate project.

## 🎯 Daily Development Workflow

### 1. Starting Your Day

```bash
# Update main branch
git checkout main
git pull origin main

# Create feature branch
git checkout -b feature/your-feature-name

# Install latest dependencies
npm install

# Start development server
npm start
```

### 2. During Development

```bash
# Run tests for your changes
npm test

# Check code quality
npm run lint

# Format code
npm run format

# View changes that affect other projects
nx affected:build --base=main
```

### 3. Before Committing

```bash
# Format all changes
npm run format

# Fix linting issues
npx eslint --fix "**/*.ts"

# Run tests
npm test

# Review changes
git diff
git status
```

## 🔄 Creating a Pull Request

### 1. Prepare Your Branch

```bash
# Ensure main is up to date
git fetch origin
git rebase origin/main

# Run full test suite
npm run affected:test --base=origin/main

# Build affected projects
npm run affected:build --base=origin/main
```

### 2. Push and Create PR

```bash
# Push to your fork
git push origin feature/your-feature-name

# Create PR on GitHub with:
# - Clear title describing the change
# - Description of what changed and why
# - Link to related issues
# - Screenshots if UI changes
```

### 3. Address Review Comments

```bash
# Make requested changes
git add .
git commit -m "refactor: address review comments"

# Push updates (will update PR automatically)
git push origin feature/your-feature-name
```

## 📦 Working with Multiple Apps

### Generate and Work on Apps

```bash
# Generate new app
nx generate @nrwl/angular:app my-app

# Serve specific app
nx serve my-app

# Build specific app
nx build my-app --configuration production

# Test specific app
nx test my-app
```

### Share Code Between Apps

```bash
# Create shared library
nx generate @nrwl/angular:lib shared-feature --directory=libs/shared

# Use in app
# apps/my-app/src/app/app.component.ts
import { SharedFeatureComponent } from '@angular-boilerplate/shared-feature';
```

## 📚 Working with Libraries

### Library Development Workflow

```bash
# Generate library with routing
nx generate @nrwl/angular:lib feature-dashboard --directory=libs/features --routing

# Add component to library
nx generate @nrwl/angular:component dashboard --project=feature-dashboard

# Test library
nx test feature-dashboard

# Build library
nx build feature-dashboard

# Lint library
nx lint feature-dashboard
```

### Using Libraries in Apps

```typescript
// apps/main-app/src/app/app.component.ts
import { FeatureDashboardComponent } from '@angular-boilerplate/feature-dashboard';
import { SharedUiHeaderComponent } from '@angular-boilerplate/shared/ui';

@Component({
  selector: 'app-root',
  standalone: true,
  imports: [FeatureDashboardComponent, SharedUiHeaderComponent],
  template: `
    <app-ui-header></app-ui-header>
    <app-dashboard></app-dashboard>
  `,
})
export class AppComponent {}
```

## 🧪 Testing Strategies

### Unit Testing

```bash
# Run all tests
npm test

# Run tests in watch mode
npm test -- --watch

# Run tests for specific project
nx test my-app

# Generate coverage report
npm test -- --code-coverage
```

### Test File Structure

```typescript
// src/app/services/user.service.spec.ts
import { TestBed } from '@angular/core/testing';
import { UserService } from './user.service';

describe('UserService', () => {
  let service: UserService;

  beforeEach(() => {
    TestBed.configureTestingModule({});
    service = TestBed.inject(UserService);
  });

  it('should be created', () => {
    expect(service).toBeTruthy();
  });

  it('should fetch users', () => {
    // Test implementation
  });
});
```

### E2E Testing

```bash
# Run e2e tests
nx e2e my-app-e2e

# Run e2e tests in headed mode
nx e2e my-app-e2e --headed

# Debug e2e tests
nx e2e my-app-e2e --headed --no-watch
```

## 🚀 Deployment Workflows

### Local Build Testing

```bash
# Build for production
npm run build -- --configuration production

# Serve production build locally
npx http-server dist/apps/main-app -p 8080 -c-1
```

### Staging Deployment

```bash
# Build for staging
npm run build -- --configuration staging

# Deploy to staging
# (Usually automated via GitHub Actions)
git push origin feature/branch
# PR merge to staging branch triggers deployment
```

### Production Deployment

```bash
# Merge to main branch
git checkout main
git merge feature/branch

# Push to trigger production build
git push origin main

# GitHub Actions automatically:
# 1. Runs tests
# 2. Builds for production
# 3. Deploys to production
```

## 🔍 Debugging

### Debug in Browser

```bash
# Start dev server
npm start

# Open Chrome DevTools (F12)
# Sources tab to view breakpoints
# Network tab to inspect API calls
```

### Angular DevTools

```bash
# Install extension (Chrome/Firefox)
# Right-click → Inspect → Angular tab
# View component tree
# Inspect component properties
# Change component state in real-time
```

### VS Code Debugging

```json
// .vscode/launch.json
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "Chrome",
      "type": "chrome",
      "request": "launch",
      "url": "http://localhost:4200",
      "webRoot": "${workspaceFolder}",
      "sourceMapPathOverrides": {
        "webpack:/*": "${webRoot}/*",
        "/./*": "${webRoot}/src/*",
        "/src/*": "${webRoot}/src/*",
        "/*": "*",
        "/./~/*": "${webRoot}/node_modules/*"
      }
    }
  ]
}
```

## 📊 Analyzing Project Impact

### Understand Dependencies

```bash
# View dependency graph
npm run dep-graph

# See affected projects
nx affected:apps --base=main
nx affected:libs --base=main

# Build only affected
nx affected:build --base=main

# Test only affected
nx affected:test --base=main
```

### Performance Analysis

```bash
# Build statistics
nx build my-app --stats-json

# Analyze bundle
npx webpack-bundle-analyzer dist/apps/my-app/stats.json
```

## 🐛 Common Issues and Solutions

### Issue: Port 4200 Already in Use

```bash
# Kill process using port 4200
# macOS/Linux
lsof -i :4200 | grep LISTEN | awk '{print $2}' | xargs kill -9

# Windows
netstat -ano | findstr :4200
taskkill /PID <PID> /F

# Or use different port
nx serve my-app --port 4300
```

### Issue: Module Not Found

```bash
# Clear Nx cache
nx reset

# Reinstall dependencies
rm -rf node_modules
npm install
```

### Issue: TypeScript Errors

```bash
# Check TypeScript version
npx tsc --version

# Rebuild TypeScript
npm run build

# Verify tsconfig paths
cat tsconfig.base.json | grep -A 20 '"paths"'
```

## 📝 Git Workflow

### Branch Naming Convention

```
feature/add-user-authentication
fix/resolve-login-bug
docs/update-readme
refactor/improve-performance
test/add-unit-tests
chore/update-dependencies
```

### Commit Message Convention

```
feat: add user authentication module
fix: resolve dropdown menu alignment issue
docs: update installation instructions
style: reformat component styles
refactor: optimize database queries
test: add tests for user service
chore: update dependencies
```

### Useful Git Commands

```bash
# Squash commits
git rebase -i HEAD~3

# Amend last commit
git commit --amend --no-edit

# Revert commit
git revert <commit-hash>

# Cherry-pick commit
git cherry-pick <commit-hash>

# View commit history
git log --oneline --graph --all

# Stash changes
git stash
git stash pop
```

---

For more help, refer to:
- [Angular Documentation](https://angular.io/docs)
- [Nx Documentation](https://nx.dev/docs)
- [Git Documentation](https://git-scm.com/doc)
