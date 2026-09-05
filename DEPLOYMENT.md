# Deployment Guide

This guide covers deployment strategies for the Angular Boilerplate project.

## Pre-Deployment Checklist

- [ ] All tests passing: \`npm test\`
- [ ] No linting errors: \`npm run lint\`
- [ ] Build successful: \`npm run build\`
- [ ] Code review completed
- [ ] Environment variables configured
- [ ] Security checks passed
- [ ] Documentation updated

## Building for Production

### Local Build

\`\`\`bash
# Build all projects
npm run build -- --configuration production

# Build specific app
nx build main-app --configuration production

# Analyze bundle size
nx build main-app --configuration production --stats-json
npx webpack-bundle-analyzer dist/apps/main-app/stats.json
\`\`\`

## Deployment Platforms

### Vercel

1. **Setup**
   \`\`\`bash
   npm i -g vercel
   vercel login
   vercel link
   \`\`\`

2. **Configure vercel.json**
   \`\`\`json
   {
     "buildCommand": "npm run build -- --configuration production",
     "outputDirectory": "dist/apps/main-app",
     "framework": "other"
   }
   \`\`\`

3. **Deploy**
   \`\`\`bash
   vercel --prod
   \`\`\`

### Netlify

1. **Setup**
   \`\`\`bash
   npm i -g netlify-cli
   netlify login
   netlify init
   \`\`\`

2. **Configure netlify.toml**
   \`\`\`toml
   [build]
   command = "npm run build -- --configuration production"
   publish = "dist/apps/main-app"
   \`\`\`

3. **Deploy**
   \`\`\`bash
   netlify deploy --prod
   \`\`\`

### Docker

\`\`\`dockerfile
FROM node:18-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build -- --configuration production

FROM node:18-alpine
WORKDIR /app
RUN npm install -g http-server
COPY --from=builder /app/dist/apps/main-app ./dist
EXPOSE 8080
CMD ["http-server", "dist", "-p", "8080"]
\`\`\`

## GitHub Actions Workflows

Automatic CI/CD is configured in `.github/workflows/`:
- **ci-cd.yml** - Tests and linting
- **build-deploy.yml** - Build and deployment
- **dependency-check.yml** - Weekly updates

For detailed deployment instructions, see the full DEPLOYMENT.md guide.
