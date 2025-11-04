# Ghost Deployment Guide for Render

This guide walks you through deploying Ghost to Render.com.

## Prerequisites

- A Render.com account
- A Ghost site repository on GitHub
- SMTP credentials for email (e.g., SendGrid, Mailgun, Gmail)
- (Optional) AWS S3 bucket for file storage

## Quick Start

### 1. Fork and Push Repository

Ensure your Ghost repository is on GitHub with all changes committed.

### 2. Create a New Web Service on Render

1. Go to [Render Dashboard](https://dashboard.render.com/)
2. Click **"New +"** → **"Blueprint"**
3. Connect your GitHub repository
4. Render will auto-detect the `render.yaml` file

### 3. Configure Environment Variables

The following environment variables are **required**:

#### Database (Auto-configured if using Render's managed database)
- `database__client` = `mysql`
- `database__connection__host` (from database)
- `database__connection__port` (from database)
- `database__connection__user` (from database)
- `database__connection__password` (from database)
- `database__connection__database` (from database)

#### Site Configuration
- `url` = Your site URL (e.g., `https://your-site.onrender.com`)
- `NODE_ENV` = `production`

#### Email Configuration (Required for user invites, password resets, etc.)

**Option 1: SendGrid**
```bash
mail__transport=SMTP
mail__options__service=SendGrid
mail__options__host=smtp.sendgrid.net
mail__options__port=587
mail__options__auth__user=apikey
mail__options__auth__pass=YOUR_SENDGRID_API_KEY
```

**Option 2: Mailgun**
```bash
mail__transport=SMTP
mail__options__host=smtp.mailgun.org
mail__options__port=587
mail__options__auth__user=YOUR_MAILGUN_SMTP_LOGIN
mail__options__auth__pass=YOUR_MAILGUN_SMTP_PASSWORD
```

**Option 3: Gmail (Not recommended for production)**
```bash
mail__transport=SMTP
mail__options__service=Gmail
mail__options__auth__user=your-email@gmail.com
mail__options__auth__pass=your-app-password
```

### 4. Configure File Storage (Important!)

**⚠️ IMPORTANT:** Render's disk storage is ephemeral and resets on each deploy.

#### Option A: Local Storage (Development Only)
```bash
storage__active=local
```
**Not recommended for production** - files will be lost on redeploy!

#### Option B: AWS S3 (Recommended for Production)

1. Install S3 storage adapter in `ghost/core/package.json`:
```json
"dependencies": {
  "ghost-storage-adapter-s3": "^2.8.0"
}
```

2. Set environment variables:
```bash
storage__active=s3
storage__s3__accessKeyId=YOUR_AWS_ACCESS_KEY_ID
storage__s3__secretAccessKey=YOUR_AWS_SECRET_ACCESS_KEY
storage__s3__region=us-east-1
storage__s3__bucket=your-bucket-name
storage__s3__assetHost=https://your-bucket.s3.amazonaws.com
# Optional CloudFront:
# storage__s3__assetHost=https://your-cloudfront-domain.cloudfront.net
```

3. Create the adapter directory:
```bash
mkdir -p ghost/core/content/adapters/storage/s3
cp node_modules/ghost-storage-adapter-s3/* ghost/core/content/adapters/storage/s3/
```

### 5. Deploy

1. Click **"Create Blueprint Instance"**
2. Wait for the build to complete (5-10 minutes for first deploy)
3. Once deployed, visit your site URL
4. Navigate to `/ghost` to complete setup

## Post-Deployment Setup

### Initial Admin Setup
1. Visit `https://your-site.onrender.com/ghost`
2. Create your admin account
3. Configure your site settings

### Custom Domain
1. In Render Dashboard → Your Service → Settings
2. Add your custom domain
3. Update the `url` environment variable to match your custom domain
4. Redeploy the service

### Update Ghost Configuration
Edit any Ghost settings via Admin panel at `/ghost/settings`

## Troubleshooting

### Build Fails
- Check that Node.js version matches `engines.node` in package.json (v22.13.1)
- Ensure all submodules are initialized
- Check build logs for specific errors

### Database Connection Issues
- Verify database environment variables are correct
- Ensure database service is running
- Check database connection logs

### Email Not Sending
- Verify SMTP credentials are correct
- Check that port 587 (or 465) is accessible
- Test with a simpler SMTP service first

### Images/Files Not Persisting
- This is expected with local storage on Render
- Switch to S3 or another external storage solution

### Application Crashes on Startup
- Check logs: `yarn render:start` locally
- Verify all required environment variables are set
- Ensure database migrations completed successfully

## Environment Variables Reference

### Required Variables
```bash
# Site
url=https://your-site.onrender.com
NODE_ENV=production

# Database
database__client=mysql
database__connection__host=
database__connection__port=
database__connection__user=
database__connection__password=
database__connection__database=

# Email
mail__transport=SMTP
mail__options__service=
mail__options__host=
mail__options__port=
mail__options__auth__user=
mail__options__auth__pass=
```

### Optional Variables
```bash
# Admin URL (if different from main site)
admin__url=https://admin.yoursite.com

# Privacy
privacy__useUpdateCheck=false

# Performance
database__pool__min=2
database__pool__max=10

# Security
paths__contentPath=/opt/render/project/src/ghost/core/content
```

## Maintenance

### Updating Ghost
1. Pull latest changes from Ghost repository
2. Run `yarn install`
3. Commit changes
4. Push to GitHub
5. Render will auto-deploy

### Backups
- **Database**: Use Render's database backup feature
- **Content**: If using S3, it's automatically backed up
- **Code**: Keep your repository updated

### Monitoring
- Use Render's built-in metrics
- Set up uptime monitoring (e.g., UptimeRobot)
- Configure Ghost's built-in monitoring in admin panel

## Performance Optimization

### Caching
Configure Redis for caching (optional):
```bash
cache__cache-control__max-age=31536000
```

### CDN
Use Cloudflare or similar CDN in front of your Render service for better performance.

## Support

- [Ghost Documentation](https://ghost.org/docs/)
- [Render Documentation](https://render.com/docs)
- [Ghost Forum](https://forum.ghost.org/)

## Cost Estimate (Render)

- **Web Service (Starter)**: $7/month
- **PostgreSQL/MySQL (Starter)**: $7/month
- **Persistent Disk (10GB)**: ~$1/month
- **Total**: ~$15/month

For production sites, consider upgrading to Standard plans for better performance.

