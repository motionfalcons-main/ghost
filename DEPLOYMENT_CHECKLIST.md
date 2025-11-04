# Ghost Render Deployment Checklist

Use this checklist to ensure your Ghost deployment is production-ready.

## Pre-Deployment

### Repository Setup
- [ ] All code changes committed and pushed to GitHub
- [ ] Git submodules initialized (`git submodule update --init`)
- [ ] `render.yaml` file reviewed and customized
- [ ] `.renderignore` configured

### Render Account Setup
- [ ] Render account created and verified
- [ ] GitHub repository connected to Render
- [ ] Blueprint detected from `render.yaml`

### Database Setup
- [ ] MySQL database created in Render
- [ ] Database name noted: `ghost_production`
- [ ] Database credentials saved securely

### Email Service Setup
- [ ] Email service provider chosen (SendGrid, Mailgun, etc.)
- [ ] SMTP credentials obtained
- [ ] Test email sent to verify credentials work

### Storage Setup (Critical!)
- [ ] Decision made: S3 or local storage
- [ ] If S3: AWS account created
- [ ] If S3: S3 bucket created with appropriate permissions
- [ ] If S3: IAM user created with S3 access
- [ ] If S3: Access keys generated and saved securely

## Deployment Configuration

### Required Environment Variables
- [ ] `NODE_ENV=production`
- [ ] `url=https://your-site.onrender.com`
- [ ] Database variables configured (or auto-configured)
- [ ] Mail transport configured
- [ ] Mail SMTP credentials added

### Optional But Recommended
- [ ] `storage__active=s3` (if using S3)
- [ ] S3 credentials configured (if using S3)
- [ ] `privacy__useUpdateCheck=false`
- [ ] Database pool settings configured

### Review Configuration
- [ ] All environment variables have correct syntax (`key__subkey=value`)
- [ ] No spaces around `=` signs
- [ ] Sensitive values marked as "Secret" in Render
- [ ] URL matches your intended domain

## Deployment

### Initial Deploy
- [ ] Blueprint deployed in Render
- [ ] Build completed successfully (check logs)
- [ ] Database migrations ran successfully
- [ ] Service started without errors
- [ ] Health check passing

### Post-Deploy Verification
- [ ] Site accessible at the deployed URL
- [ ] `/ghost` admin accessible
- [ ] Admin account created
- [ ] Test post created
- [ ] Test email sent (check spam folder)
- [ ] Images uploaded successfully
- [ ] Images persist after redeploy (if using S3)

## Production Hardening

### Security
- [ ] Admin password is strong and unique
- [ ] Two-factor authentication enabled
- [ ] SMTP credentials are secure
- [ ] AWS keys (if using) have minimal permissions
- [ ] Database credentials rotated from defaults

### Performance
- [ ] Custom domain configured
- [ ] SSL certificate auto-issued by Render
- [ ] CDN configured (optional: Cloudflare)
- [ ] Database indexed properly
- [ ] Redis caching enabled (optional)

### Monitoring
- [ ] Uptime monitoring configured (e.g., UptimeRobot)
- [ ] Error tracking configured (optional: Sentry)
- [ ] Log aggregation reviewed
- [ ] Render metrics dashboard reviewed

### Backup
- [ ] Database backup schedule enabled in Render
- [ ] S3 versioning enabled (if using S3)
- [ ] Content export tested from Ghost admin
- [ ] Recovery procedure documented

## Post-Launch

### Site Configuration
- [ ] Site title and description updated
- [ ] Theme customized
- [ ] Navigation menu configured
- [ ] Social media links added
- [ ] Newsletter settings configured (if using)

### SEO
- [ ] Meta description set
- [ ] Social cards configured
- [ ] Sitemap accessible at `/sitemap.xml`
- [ ] Robots.txt configured
- [ ] Google Search Console submitted

### Content
- [ ] Sample posts deleted or unpublished
- [ ] First real post published
- [ ] About page created
- [ ] Contact information added

### Integrations
- [ ] Analytics configured (Google Analytics, Plausible, etc.)
- [ ] Comment system integrated (if desired)
- [ ] Email newsletter platform connected (if using)
- [ ] Stripe connected (if using paid memberships)

## Ongoing Maintenance

### Regular Tasks
- [ ] Monitor site performance weekly
- [ ] Review error logs weekly
- [ ] Update Ghost when new versions release
- [ ] Rotate credentials quarterly
- [ ] Test backups quarterly
- [ ] Review and respond to comments
- [ ] Check uptime reports

### Incident Response Plan
- [ ] Backup restoration procedure tested
- [ ] Rollback procedure documented
- [ ] Emergency contacts list created
- [ ] Status page configured (optional)

## Common Issues Checklist

### If Site Won't Load
- [ ] Check Render logs for errors
- [ ] Verify service is "Running" in Render dashboard
- [ ] Check database connection is active
- [ ] Verify URL environment variable is correct
- [ ] Check for port binding issues

### If Admin Won't Load
- [ ] Clear browser cache
- [ ] Try incognito/private browsing
- [ ] Check `/ghost` URL is correct
- [ ] Verify admin URL environment variable (if set)
- [ ] Check for JavaScript errors in browser console

### If Emails Don't Send
- [ ] Verify SMTP credentials
- [ ] Check spam/junk folder
- [ ] Test SMTP credentials with external tool
- [ ] Review mail logs in Render
- [ ] Verify mail port isn't blocked

### If Images Don't Persist
- [ ] Verify using S3 (not local storage)
- [ ] Check S3 credentials are correct
- [ ] Verify S3 bucket permissions
- [ ] Test S3 upload manually
- [ ] Check storage adapter is installed

## Success Criteria

Your Ghost site is production-ready when:
- ✅ Site loads consistently at your domain
- ✅ Admin panel is accessible and secure
- ✅ Can create and publish posts
- ✅ Images upload and display correctly
- ✅ Emails send successfully
- ✅ Database backups are enabled
- ✅ Monitoring is in place
- ✅ Site survives a redeploy without losing data

## Resources

- [Ghost Documentation](https://ghost.org/docs/)
- [Render Documentation](https://render.com/docs)
- [Full Deployment Guide](./RENDER_DEPLOYMENT.md)
- [Environment Variables Guide](./ENVIRONMENT_VARIABLES.md)

---

**Pro Tip:** Keep this checklist handy and work through it methodically. Most deployment issues come from skipped steps or misconfigured environment variables.

