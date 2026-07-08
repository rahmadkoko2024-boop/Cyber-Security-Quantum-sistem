# Vercel Deployment Guide - Cybersecurity Edition

## Quick Start

### Prerequisites
- Vercel account with organization setup
- GitHub token with repo access
- Environment variables configured

### Step 1: Configure Secrets in GitHub

Go to your repository settings → **Secrets and variables** → **Actions**

Add the following secrets:

```bash
VERCEL_TOKEN          # Your Vercel authentication token
VERCEL_ORG_ID         # Your Vercel organization ID
VERCEL_PROJECT_ID     # Your Vercel project ID
```

### Step 2: Vercel Token Setup

1. Visit https://vercel.com/account/tokens
2. Create a new token with name: `github-ci`
3. Set expiration to 90 days (rotate regularly)
4. Copy the token and add as `VERCEL_TOKEN` secret

### Step 3: Link Repository to Vercel

```bash
# Install Vercel CLI
npm install -g vercel

# Link your project
vercel link

# Get your IDs from .vercel/project.json
cat .vercel/project.json
```

## Deployment Branches

| Branch | Environment | Purpose |
|--------|-------------|---------|
| `main` | **Production** | Live application |
| `develop` | **Preview** | Testing & staging |

## Security Features

### 1. Vulnerability Scanning
- **Trivy**: Container & filesystem scanning
- **Semgrep**: Static analysis (SAST)
- **pip-audit**: Python dependency check
- **npm audit**: JavaScript dependency check

### 2. Automated Deployment
- ✅ Main branch → Production (Vercel)
- ✅ Develop branch → Preview deployment
- ✅ Pull requests → Automatic preview URLs

### 3. Security Headers Verification
Automatically checks for:
- `Strict-Transport-Security` (HSTS)
- `X-Content-Type-Options`
- `X-Frame-Options` (Clickjack protection)
- `Content-Security-Policy`

## Environment Variables

Create `.env.local` for local development:

```env
# Database
DATABASE_URL=postgresql://...

# API Keys (use Vercel's secrets for production)
API_SECRET_KEY=xxxx

# Security
CORS_ORIGIN=https://your-domain.com
JWT_SECRET=xxxx

# Feature Flags
ENABLE_SECURITY_HEADERS=true
LOG_SECURITY_EVENTS=true
```

## Monitoring & Logs

### View Deployment Logs
```bash
vercel logs --prod
```

### View Security Scans
GitHub → **Security** → **Code scanning alerts**

## Rollback Procedures

### Rollback to Previous Deployment
```bash
vercel rollback --prod
```

### Manual Rollback
1. Go to Vercel dashboard
2. Select your project
3. Click **Deployments**
4. Select previous version → **Promote to Production**

## Best Practices

### 1. Branch Protection Rules
- Require status checks to pass (Vercel, security scanning)
- Require PR reviews before merge
- Require code owner review
- Dismiss stale PR approvals

### 2. Secret Management
- ✅ Use Vercel's built-in secrets (not .env in repo)
- ✅ Rotate tokens every 90 days
- ✅ Use least privilege for API tokens
- ❌ Never commit secrets to Git

### 3. Deployment Safety
- ✅ Use preview deployments for testing
- ✅ Require all security scans to pass
- ✅ Document all environment variables
- ✅ Maintain staging → production workflow

### 4. Monitoring
- ✅ Enable Vercel analytics
- ✅ Set up error tracking (e.g., Sentry)
- ✅ Monitor security headers
- ✅ Review deployment logs regularly

## Troubleshooting

### Build Failed in Vercel but Works Locally
```bash
# Clear cache and rebuild
vercel build --prod

# Check environment variables in Vercel dashboard
```

### Deployment Timeout
- Check build time in Vercel settings
- Optimize large dependencies
- Use Vercel's caching features

### Security Scan Failures
- Review GitHub Security tab for alerts
- Fix vulnerabilities before merging
- Use `--allow-vulnerabilities` only in development

## Deployment Checklist

- [ ] Secrets configured in GitHub & Vercel
- [ ] Vercel project linked to repository
- [ ] Branch protection rules enabled
- [ ] Security scanning enabled
- [ ] Environment variables set
- [ ] Custom domain configured (if needed)
- [ ] SSL certificate verified
- [ ] Monitoring setup complete
- [ ] Backup/rollback plan documented

## Support & Resources

- **Vercel Docs**: https://vercel.com/docs
- **GitHub Actions**: https://docs.github.com/en/actions
- **Security Tools**:
  - Trivy: https://github.com/aquasecurity/trivy
  - Semgrep: https://semgrep.dev
  - OWASP Top 10: https://owasp.org/www-project-top-ten/

---

**Last Updated**: 2026-07-08
**Maintained By**: rahmadkoko2024-boop
