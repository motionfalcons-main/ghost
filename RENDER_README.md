# Ghost Render Deployment - Complete Package 📦

Your Ghost blog is now **ready for Render deployment**! This package includes everything you need to deploy Ghost to production.

## 📁 Files Added

### Core Configuration
- **`render.yaml`** - Render service configuration (auto-detected on deploy)
- **`.renderignore`** - Files to exclude from deployment
- **`package.json`** - Updated with `render:build` and `render:start` scripts

### Production Config Template
- **`ghost/core/config.production.json.example`** - Production configuration template

### Documentation
- **`QUICK_START_RENDER.md`** - 🚀 **START HERE** - Deploy in 15 minutes
- **`RENDER_DEPLOYMENT.md`** - Complete deployment guide
- **`ENVIRONMENT_VARIABLES.md`** - All environment variable options
- **`DEPLOYMENT_CHECKLIST.md`** - Pre/post deployment checklist

## 🎯 Quick Start (3 Steps)

### 1. Commit & Push
```bash
git add .
git commit -m "🚀 Added Render deployment configuration"
git push origin main
```

### 2. Deploy to Render
1. Go to [Render Dashboard](https://dashboard.render.com/)
2. Click **"New +"** → **"Blueprint"**
3. Select your GitHub repository
4. Click **"Apply"**

### 3. Configure Environment
Add these variables in Render → Environment:
```bash
url=https://your-site.onrender.com
mail__options__auth__pass=YOUR_SENDGRID_API_KEY
```

**Done!** Your site will be live at `https://your-site.onrender.com` in ~10 minutes.

## 📚 Documentation Guide

### For First-Time Deployers
→ Start with **`QUICK_START_RENDER.md`**

### For Detailed Setup
→ Read **`RENDER_DEPLOYMENT.md`**

### For Configuration Reference
→ See **`ENVIRONMENT_VARIABLES.md`**

### Before Going Live
→ Use **`DEPLOYMENT_CHECKLIST.md`**

## ⚙️ What's Configured

### Services (from `render.yaml`)
- ✅ **Web Service** - Ghost application (Node.js)
- ✅ **MySQL Database** - Production database (managed)
- ✅ **Persistent Disk** - 10GB for content storage

### Build Process
```bash
# Automated via render:build script
1. Install dependencies (yarn install)
2. Initialize submodules (git submodule update --init)
3. Build all packages (yarn build)
4. Start Ghost (cd ghost/core && node index.js)
```

### Environment Variables
All sensitive data configured via Render Environment Variables:
- Database credentials (auto-configured)
- Site URL
- SMTP email settings
- Optional: S3 storage, Redis cache, Stripe keys

## 🔐 Security Checklist

Before going live:
- [ ] Strong admin password set
- [ ] SMTP credentials secured
- [ ] Database password rotated
- [ ] S3 keys (if used) have minimal permissions
- [ ] SSL enabled (automatic with Render)
- [ ] 2FA enabled on admin account

## 📊 Cost Breakdown

### Starter Setup (~$15/month)
- Web Service (Starter): $7/month
- MySQL Database (Starter): $7/month  
- 10GB Disk: ~$1/month

### Optional Add-ons
- Custom domain: Free
- SSL certificate: Free (auto-provisioned)
- Redis cache: $10/month (optional)
- Extra disk space: $0.10/GB/month

### Free Tier Available
- Perfect for testing
- Limited resources
- Sleeps after inactivity

## 🚨 Important Notes

### ⚠️ File Storage Warning
**Default configuration uses local storage** which is **ephemeral on Render**.

**For production:**
- Set up AWS S3 (recommended)
- Or use another external storage service
- Otherwise, uploaded images will be lost on redeploy

See [S3 setup guide](./RENDER_DEPLOYMENT.md#option-b-aws-s3-recommended-for-production)

### ⚠️ Email Configuration Required
Ghost **requires** email configuration for:
- Admin account creation
- Password resets
- User invitations
- Newsletter sending

**Must configure** SMTP settings before deployment!

### ⚠️ Database Migrations
On first deploy, database migrations run automatically.
**Do not interrupt** the first deployment!

## 🐛 Troubleshooting Quick Reference

### Build Fails
```bash
# Check Node version matches v22.13.1
# Verify render.yaml is valid YAML
# Check build logs for specific error
```

### Site Won't Load
```bash
# Verify service status in Render dashboard
# Check logs for startup errors
# Confirm database is connected
# Verify url environment variable
```

### Email Doesn't Work
```bash
# Test SMTP credentials externally
# Check spam folder
# Verify port 587 is accessible
# Confirm mail__ env vars are correct
```

### Images Don't Persist
```bash
# Expected with local storage
# Set up S3 storage
# Or accept images reset on redeploy
```

## 🎓 Learning Resources

### Ghost Specific
- [Ghost Documentation](https://ghost.org/docs/)
- [Ghost Admin Guide](https://ghost.org/help/)
- [Ghost Forum](https://forum.ghost.org/)
- [Ghost Themes](https://ghost.org/themes/)

### Render Specific  
- [Render Docs](https://render.com/docs)
- [Render Blueprints](https://render.com/docs/infrastructure-as-code)
- [Render Environment Variables](https://render.com/docs/environment-variables)
- [Render Persistent Disks](https://render.com/docs/disks)

### Integrations
- [AWS S3 Setup](https://aws.amazon.com/s3/getting-started/)
- [SendGrid Getting Started](https://sendgrid.com/docs/for-developers/)
- [Stripe for Memberships](https://ghost.org/help/setup-members-stripe/)

## 🎉 Next Steps After Deployment

1. **Complete Admin Setup** (`/ghost` path)
2. **Configure Custom Domain** (Render settings)
3. **Set Up S3 Storage** (for production)
4. **Install a Theme** (or customize default)
5. **Configure Email Newsletter** (if using)
6. **Set Up Analytics** (Google Analytics, Plausible, etc.)
7. **Submit Sitemap to Google** (`/sitemap.xml`)
8. **Create Content!** ✍️

## 💡 Pro Tips

1. **Use staging environment** - Test changes before production
2. **Enable database backups** - Render does this automatically
3. **Set up monitoring** - Use UptimeRobot or similar
4. **Use CDN** - CloudFlare in front of Render for better performance
5. **Regular updates** - Keep Ghost up to date
6. **Test email** - Before you need it in production
7. **Document customizations** - For future reference

## 🆘 Getting Help

### Deployment Issues
1. Check logs in Render dashboard
2. Review [DEPLOYMENT_CHECKLIST.md](./DEPLOYMENT_CHECKLIST.md)
3. Search [Ghost Forum](https://forum.ghost.org/)
4. Contact [Render Support](https://render.com/support)

### Ghost Configuration
1. Review [ENVIRONMENT_VARIABLES.md](./ENVIRONMENT_VARIABLES.md)
2. Check [Ghost Documentation](https://ghost.org/docs/)
3. Ask on [Ghost Forum](https://forum.ghost.org/)

### Emergency Support
- Ghost Pro: [ghost.org/pricing/](https://ghost.org/pricing/)
- Render Support: [render.com/support](https://render.com/support)

## ✅ Success Criteria

Your deployment is successful when:
- ✅ Site loads at your URL
- ✅ Can access `/ghost` admin
- ✅ Can create and publish posts
- ✅ Images upload and display
- ✅ Emails send successfully
- ✅ Site survives a redeploy
- ✅ Database persists data
- ✅ No errors in logs

---

## 🚀 Ready to Deploy?

1. Read **[QUICK_START_RENDER.md](./QUICK_START_RENDER.md)** (15 minutes)
2. Follow the checklist in **[DEPLOYMENT_CHECKLIST.md](./DEPLOYMENT_CHECKLIST.md)**
3. Deploy and celebrate! 🎉

**Good luck with your Ghost blog!** 👻✨

---

*Last updated: $(date +%Y-%m-%d)*
*Ghost Version: 6.6.0*
*Node Version Required: 22.13.1*

