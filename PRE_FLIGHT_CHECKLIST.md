# ✈️ Pre-Flight Checklist - Ghost on Render

**Before deploying to Render, verify all items below:**

---

## 📦 Repository Status

- [ ] All changes committed locally
- [ ] Repository pushed to GitHub
- [ ] Git submodules initialized (`git submodule update --init`)
- [ ] No merge conflicts
- [ ] On main/master branch

**Quick check:**
```bash
git status
git submodule status
```

---

## 🔑 Credentials Ready

### Required Immediately:
- [ ] **Render Account** - [Sign up](https://dashboard.render.com/register)
- [ ] **GitHub Account** - Repository access confirmed
- [ ] **Email Service** (Choose one):
  - [ ] SendGrid API Key ([Get it here](https://app.sendgrid.com/settings/api_keys))
  - [ ] Mailgun SMTP credentials
  - [ ] Gmail App Password (testing only)

### Recommended for Production:
- [ ] **AWS Account** - For S3 storage
- [ ] **S3 Bucket Created** - Name: `ghost-[your-site]`
- [ ] **IAM User Created** - With S3 access only
- [ ] **AWS Access Keys** - Saved securely

---

## 📝 Configuration Files

- [ ] `render.yaml` exists in repository root
- [ ] `package.json` has `render:build` script
- [ ] `package.json` has `render:start` script
- [ ] Node.js version specified: `^22.13.1`

**Verify:**
```bash
cat render.yaml | grep "buildCommand"
cat render.yaml | grep "startCommand"
cat ghost/core/package.json | grep "\"node\""
```

---

## 🌐 DNS & Domain (Optional)

If using custom domain:
- [ ] Domain purchased and accessible
- [ ] DNS provider login credentials ready
- [ ] Know how to add CNAME records
- [ ] Willing to wait 5-10 mins for SSL provisioning

---

## 💡 Knowledge Check

Before proceeding, you should understand:

- [ ] **Images with local storage will be lost on redeploy** (use S3 for production)
- [ ] **First deployment takes 10-15 minutes** (subsequent deploys: 5-7 mins)
- [ ] **Free tier has limitations** (services spin down after inactivity)
- [ ] **Starter plan costs ~$15/month** (web + database + disk)
- [ ] **PostgreSQL is the database** (Ghost auto-detects and uses it)

---

## 🧪 Pre-Deployment Test (Optional but Recommended)

Test locally before deploying:

```bash
# 1. Install dependencies
yarn install

# 2. Initialize submodules
git submodule update --init

# 3. Build Ghost
yarn build

# 4. Run locally
yarn dev

# 5. Visit http://localhost:2368
# 6. Verify admin works at http://localhost:2368/ghost
```

**Local build successful?**
- [ ] No build errors
- [ ] Site loads locally
- [ ] Admin panel accessible
- [ ] No console errors

---

## 🚀 Ready to Deploy Checklist

Final verification before clicking "Apply Blueprint":

### Repository:
- [ ] ✅ All code pushed to GitHub
- [ ] ✅ render.yaml in root directory
- [ ] ✅ No pending changes (`git status` is clean)

### Credentials:
- [ ] ✅ Email service API key/credentials ready to paste
- [ ] ✅ AWS S3 keys ready (if using S3)

### Render Account:
- [ ] ✅ Logged into Render Dashboard
- [ ] ✅ Credit card added (if using paid plan)
- [ ] ✅ Ready to create Blueprint

### You Understand:
- [ ] ✅ Deployment takes 10-15 minutes first time
- [ ] ✅ You'll need to configure environment variables after blueprint applies
- [ ] ✅ Images need S3 to persist across deploys
- [ ] ✅ You'll create admin account at `/ghost` after deployment

---

## 📋 Environment Variables You'll Need to Set

Have these values ready to copy-paste into Render:

```bash
# REQUIRED IMMEDIATELY:

url = https://[your-service-name].onrender.com
# ⚠️ You'll get this URL after Blueprint is created

mail__options__service = SendGrid
# (or Mailgun, Gmail, etc.)

mail__options__host = smtp.sendgrid.net
# (or your email provider's SMTP host)

mail__options__auth__user = apikey
# (SendGrid uses "apikey", others use email/username)

mail__options__auth__pass = [YOUR_API_KEY_HERE]
# ⚠️ MARK AS SECRET IN RENDER!
```

```bash
# RECOMMENDED (if using S3):

storage__active = s3

storage__s3__accessKeyId = [YOUR_AWS_ACCESS_KEY_ID]
# ⚠️ MARK AS SECRET IN RENDER!

storage__s3__secretAccessKey = [YOUR_AWS_SECRET_KEY]
# ⚠️ MARK AS SECRET IN RENDER!

storage__s3__region = us-west-2
# (or your preferred AWS region)

storage__s3__bucket = ghost-your-site
# (your S3 bucket name)

storage__s3__assetHost = https://ghost-your-site.s3.amazonaws.com
# (or CloudFront URL)
```

---

## ⏱️ Time Budget

Allocate time for each phase:

- **Repository Push**: 2-5 minutes
- **Blueprint Setup**: 5 minutes
- **First Deploy**: 10-15 minutes
- **Environment Config**: 5-10 minutes
- **Redeploy**: 5-7 minutes
- **Ghost Setup**: 5 minutes
- **Testing**: 10 minutes
- **Custom Domain** (optional): 15-30 minutes

**Total Time: 45-60 minutes** for complete deployment

---

## 🆘 Escape Hatches

If something goes wrong:

1. **Build fails?** 
   - Check logs in Render Dashboard
   - Verify Node.js version
   - Ensure git submodules are initialized

2. **Need to start over?**
   - Delete services in Render Dashboard
   - Delete database in Render Dashboard
   - Re-run Blueprint

3. **Stuck?**
   - Read [FRESH_RENDER_DEPLOYMENT.md](./FRESH_RENDER_DEPLOYMENT.md)
   - Check [Troubleshooting section](./FRESH_RENDER_DEPLOYMENT.md#-troubleshooting)
   - Visit [Ghost Forum](https://forum.ghost.org/)

---

## ✅ All Set? Let's Deploy!

If you checked all relevant boxes above:

1. **Open**: [FRESH_RENDER_DEPLOYMENT.md](./FRESH_RENDER_DEPLOYMENT.md)
2. **Follow**: Steps 1-8 carefully
3. **Celebrate**: Your Ghost blog will be live! 🎉

---

## 📞 Emergency Contacts

Keep these handy:

- 📖 **Deployment Guide**: [FRESH_RENDER_DEPLOYMENT.md](./FRESH_RENDER_DEPLOYMENT.md)
- 🐛 **Troubleshooting**: See deployment guide
- 💬 **Ghost Forum**: https://forum.ghost.org/
- 📘 **Render Docs**: https://render.com/docs
- 🎫 **Render Support**: support@render.com

---

**Good luck!** 🚀

You've got this! Follow the steps carefully, and you'll have a production Ghost blog running in under an hour.

