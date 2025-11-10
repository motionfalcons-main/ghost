# 🚀 Fresh Ghost Deployment on Render - Complete Guide

**Status:** Ready for fresh deployment! All previous services have been cleaned up.

## 📋 Pre-Flight Checklist

Before starting, ensure you have:
- ✅ GitHub account with this repository pushed
- ✅ Render account ([Sign up here](https://dashboard.render.com/register))
- ✅ Email service credentials ready (SendGrid recommended - free tier available)
- ✅ AWS S3 bucket (optional but recommended for production)

---

## 🎯 Deployment Steps

### Step 1: Push Your Code to GitHub (2 minutes)

Make sure all your changes are committed and pushed:

```bash
# Check if you have uncommitted changes
git status

# If you have changes, commit them
git add .
git commit -m "✨ Ready for fresh Render deployment"
git push origin main
```

### Step 2: Deploy to Render Using Blueprint (5 minutes)

1. **Go to Render Dashboard**: [https://dashboard.render.com/](https://dashboard.render.com/)

2. **Create New Blueprint**:
   - Click **"New +"** in the top right
   - Select **"Blueprint"**
   - Choose **"Connect a repository"**

3. **Connect Your Repository**:
   - Select this repository from GitHub
   - Render will automatically detect the `render.yaml` file
   - Click **"Apply Blueprint"**

4. **Render will create**:
   - ✅ Web service named `ghost` (Node.js)
   - ✅ PostgreSQL database named `ghost-db`
   - ✅ Persistent disk storage (10GB) at `/opt/render/project/src/ghost/core/content`

### Step 3: Configure Environment Variables (5 minutes)

After the blueprint is applied, you need to configure the required environment variables.

**Go to**: Render Dashboard → `ghost` service → **Environment** tab

#### Required Variables to Set:

```bash
# 1. Your Site URL (CRITICAL!)
url
# Value: https://YOUR-SERVICE-NAME.onrender.com
# (Get this from your service URL after deployment starts)
# Example: https://ghost-xyz1.onrender.com

# 2. Email Configuration (Choose one option)

# Option A: SendGrid (Recommended - 100 free emails/day)
mail__options__service
# Value: SendGrid

mail__options__host
# Value: smtp.sendgrid.net

mail__options__auth__user
# Value: apikey

mail__options__auth__pass
# Value: YOUR_SENDGRID_API_KEY
# (Get from: https://app.sendgrid.com/settings/api_keys)
# ⚠️ Mark as SECRET in Render!

# Option B: Mailgun
mail__options__service
# Value: Mailgun

mail__options__host
# Value: smtp.mailgun.org

mail__options__auth__user
# Value: YOUR_MAILGUN_SMTP_LOGIN

mail__options__auth__pass
# Value: YOUR_MAILGUN_SMTP_PASSWORD
# ⚠️ Mark as SECRET in Render!

# Option C: Gmail (Testing only, not recommended for production)
mail__options__service
# Value: Gmail

mail__options__host
# Value: smtp.gmail.com

mail__options__auth__user
# Value: your-email@gmail.com

mail__options__auth__pass
# Value: YOUR_APP_PASSWORD (not your regular Gmail password!)
# (Enable 2FA and create app password at: https://myaccount.google.com/apppasswords)
# ⚠️ Mark as SECRET in Render!
```

#### Optional but Recommended (Production):

```bash
# S3 Storage Configuration (Highly Recommended!)
# Without S3, uploaded images will be LOST on every redeploy!

storage__active
# Value: s3

storage__s3__accessKeyId
# Value: YOUR_AWS_ACCESS_KEY_ID
# ⚠️ Mark as SECRET in Render!

storage__s3__secretAccessKey
# Value: YOUR_AWS_SECRET_ACCESS_KEY
# ⚠️ Mark as SECRET in Render!

storage__s3__region
# Value: us-east-1 (or your preferred region)

storage__s3__bucket
# Value: your-ghost-bucket-name

storage__s3__assetHost
# Value: https://your-bucket-name.s3.amazonaws.com
# Or with CloudFront: https://your-cloudfront-domain.cloudfront.net

# Privacy Settings
privacy__useUpdateCheck
# Value: false

# Database Connection Pool (for better performance)
database__pool__min
# Value: 2

database__pool__max
# Value: 10
```

**Important**: After adding/updating environment variables, click **"Save Changes"** to trigger a redeploy!

### Step 4: Get Your SendGrid API Key (If using SendGrid)

1. Go to [SendGrid Sign Up](https://signup.sendgrid.com/)
2. Create a free account
3. Verify your email address
4. Navigate to: **Settings → API Keys**
5. Click **"Create API Key"**
6. Name it: `Ghost-Render-Production`
7. Select **"Restricted Access"** → Enable **"Mail Send"** permission only
8. Click **"Create & View"**
9. **Copy the API key** (you'll only see it once!)
10. Add it to Render as `mail__options__auth__pass`

### Step 5: Set Up AWS S3 (Recommended for Production)

If you want persistent image storage (highly recommended):

#### A. Create S3 Bucket

1. Go to [AWS S3 Console](https://console.aws.amazon.com/s3/)
2. Click **"Create bucket"**
3. Bucket name: `ghost-YOUR-SITE-NAME` (must be globally unique)
4. Region: Choose closest to Render (e.g., `us-west-2` for Oregon)
5. **Block all public access**: ❌ **UNCHECK** (we need public reads for images)
6. Click **"Create bucket"**

#### B. Configure Bucket CORS

In your bucket → **Permissions** → **CORS**, add:

```json
[
    {
        "AllowedHeaders": ["*"],
        "AllowedMethods": ["GET", "HEAD", "PUT", "POST", "DELETE"],
        "AllowedOrigins": ["*"],
        "ExposeHeaders": ["ETag"]
    }
]
```

#### C. Create IAM User

1. Go to [IAM Console](https://console.aws.amazon.com/iam/)
2. **Users** → **Add users**
3. Username: `ghost-s3-uploader`
4. Access type: ✅ **Programmatic access**
5. **Attach policies directly** → **Create policy**:

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "s3:PutObject",
                "s3:GetObject",
                "s3:PutObjectAcl",
                "s3:DeleteObject"
            ],
            "Resource": "arn:aws:s3:::YOUR-BUCKET-NAME/*"
        }
    ]
}
```

6. **Save the Access Key ID and Secret Access Key** (you won't see the secret again!)
7. Add them to Render environment variables

### Step 6: Monitor Deployment (10-15 minutes)

Watch the build logs in Render:

1. Go to: **Render Dashboard → ghost service → Logs**
2. You'll see:
   - ✅ Installing dependencies (yarn install)
   - ✅ Initializing git submodules
   - ✅ Building Ghost and all apps
   - ✅ Database migrations running
   - ✅ Server starting

**Success indicators:**
```
==> Starting service with 'yarn render:start'
Ghost is running in production...
Your Ghost site is now available on https://your-service.onrender.com
```

**First deploy takes 10-15 minutes** - subsequent deploys will be faster (5-7 minutes).

### Step 7: Complete Ghost Setup (5 minutes)

Once deployed:

1. **Visit your site**: `https://your-service-name.onrender.com`
2. **Go to admin**: `https://your-service-name.onrender.com/ghost`
3. **Create your admin account**:
   - Full name
   - Email address (make sure it's valid!)
   - Strong password (save it in a password manager!)
4. **Configure site basics**:
   - Site title
   - Site description
5. **Test email**:
   - Try password reset or invite a team member
   - Check spam folder if email doesn't arrive
6. **Upload a test image** to verify storage works

### Step 8: Custom Domain (Optional)

To use your own domain instead of `.onrender.com`:

1. **In Render Dashboard**:
   - Go to your Ghost service → **Settings**
   - Scroll to **Custom Domain**
   - Add your domain (e.g., `blog.yourdomain.com`)

2. **In your DNS provider**:
   - Add CNAME record:
     - Name: `blog` (or `@` for root domain)
     - Value: `your-service.onrender.com`

3. **Update URL in Render**:
   - Go to **Environment** tab
   - Update `url` variable to: `https://blog.yourdomain.com`
   - Save changes (will trigger redeploy)

4. **Wait for SSL**: Render automatically provisions SSL certificate (takes 5-10 minutes)

---

## ✅ Post-Deployment Verification

Check these items to ensure everything is working:

- [ ] Site loads at your URL
- [ ] Admin accessible at `/ghost`
- [ ] Can log in with admin account
- [ ] Create a test post
- [ ] Upload an image to the test post
- [ ] Publish the test post
- [ ] View the published post on the frontend
- [ ] Send a test email (password reset or team invite)
- [ ] Check email arrives (check spam folder)
- [ ] If using S3: Redeploy and verify images still load
- [ ] If using local storage: Understand images will be lost on redeploy

---

## 🐛 Troubleshooting

### Build Fails

**Error: Node version mismatch**
```
Solution: Render should auto-detect Node.js v22.13.1 from package.json
If not, add environment variable:
NODE_VERSION = 22.13.1
```

**Error: Git submodule issues**
```bash
# Run locally to fix:
git submodule update --init --recursive
git add .
git commit -m "Fix submodules"
git push
```

### Site Won't Load

**"Application failed to respond"**
```
1. Check Render logs for errors
2. Verify DATABASE environment variables are set
3. Ensure url environment variable matches your actual URL
4. Check that PORT is set to 10000
```

### Admin Panel 404 or Blank

**Admin won't load at /ghost**
```
1. Wait 2-3 minutes after deploy completes
2. Hard refresh (Ctrl+Shift+R or Cmd+Shift+R)
3. Try incognito/private browsing
4. Check browser console for JavaScript errors
5. Verify build completed successfully in logs
```

### Emails Not Sending

**Emails never arrive**
```
1. Check spam/junk folder
2. Verify SMTP credentials are correct
3. Ensure mail__options__auth__pass is marked as SECRET
4. Test SMTP credentials with a tool like Mailtrap
5. Check Render logs for mail errors
6. Try a different email service (Gmail for testing)
```

### Images Don't Persist After Redeploy

**Images disappear after deployment**
```
This is EXPECTED with local storage!

Solution: Set up S3 storage (see Step 5)
- Local storage is ephemeral on Render
- Use S3, CloudFlare R2, or similar for production
```

### Database Connection Errors

**"Unable to acquire a connection"**
```
1. Verify ghost-db database is running
2. Check database credentials in environment variables
3. Ensure database__client is set to "postgres"
4. Check database allows connections from Render
5. Try restarting the database in Render dashboard
```

---

## 💰 Cost Breakdown

**Starter Plan (Recommended for small-medium blogs):**
- Web Service: **$7/month**
- PostgreSQL Database: **$7/month**
- Persistent Disk (10GB): **~$1/month**
- **Total: ~$15/month**

**Plus S3 Storage (optional but recommended):**
- AWS S3: **~$0.50-2/month** (depends on traffic)
- CloudFront CDN (optional): **~$1-5/month**

**Free Tier Available:**
- Perfect for testing and development
- Limited resources (will spin down after inactivity)
- Database has limited size

---

## 📊 Monitoring Your Site

### Built-in Render Monitoring

- **Metrics**: CPU, memory, requests per minute
- **Logs**: Real-time application logs
- **Events**: Deployments, restarts, health checks

### Recommended External Monitoring

1. **Uptime Monitoring**: [UptimeRobot](https://uptimerobot.com/) (free)
   - Monitor every 5 minutes
   - Get alerts if site goes down

2. **Analytics**: [Plausible](https://plausible.io/) or [Google Analytics](https://analytics.google.com/)
   - Track visitors, page views
   - Understand your audience

3. **Error Tracking**: [Sentry](https://sentry.io/) (optional)
   - Catch JavaScript errors
   - Monitor performance

---

## 🔄 Updating Ghost

When a new Ghost version is released:

```bash
# 1. Pull latest changes
git pull origin main

# 2. Install dependencies
yarn install

# 3. Test locally (optional but recommended)
yarn dev

# 4. Commit and push
git add .
git commit -m "Update to Ghost v5.x.x"
git push origin main

# Render will automatically deploy!
```

---

## 🔐 Security Best Practices

1. **Strong Admin Password**: Use a password manager
2. **Enable 2FA**: Settings → Staff → Your Account → Two-Factor Authentication
3. **Regular Updates**: Keep Ghost updated with latest security patches
4. **Secure SMTP Credentials**: Always mark as SECRET in Render
5. **AWS Keys**: Use IAM user with minimal permissions (only S3 access)
6. **Database Backups**: Enable in Render (automatic daily backups on paid plans)
7. **Custom Domain + SSL**: Use HTTPS (automatic with Render)

---

## 📚 Additional Resources

- 📖 [Ghost Documentation](https://ghost.org/docs/)
- 🎨 [Ghost Themes](https://ghost.org/marketplace/)
- 💬 [Ghost Forum](https://forum.ghost.org/)
- 🔧 [Render Docs](https://render.com/docs)
- ✅ [Deployment Checklist](./DEPLOYMENT_CHECKLIST.md)
- 🔑 [Environment Variables Guide](./ENVIRONMENT_VARIABLES.md)

---

## 🎉 Success Indicators

Your Ghost deployment is production-ready when:

- ✅ Site loads consistently at your domain
- ✅ Admin panel accessible and secure
- ✅ Can create, edit, and publish posts
- ✅ Images upload and display correctly
- ✅ Images persist after redeployment (with S3)
- ✅ Emails send successfully (no spam folder issues)
- ✅ SSL certificate is active (HTTPS)
- ✅ Database backups are enabled
- ✅ Monitoring/alerts are configured
- ✅ Custom domain is working (if applicable)

---

## 🆘 Getting Help

If you run into issues:

1. **Check this guide first** - most issues are covered in troubleshooting
2. **Read the logs** - Render logs show detailed error messages
3. **Search Ghost Forum** - [forum.ghost.org](https://forum.ghost.org/)
4. **Render Support** - [render.com/docs](https://render.com/docs)
5. **GitHub Issues** - Check if it's a known issue

---

## 📝 What's Already Done For You

This repository is pre-configured with:

✅ **render.yaml** - Blueprint for automated deployment  
✅ **Build scripts** - `yarn render:build` and `yarn render:start`  
✅ **PostgreSQL support** - Database adapter configured  
✅ **Persistent storage** - 10GB disk for content  
✅ **Environment variables** - Pre-configured structure  
✅ **Production optimizations** - Build process optimized  
✅ **Documentation** - Comprehensive guides included  

**You just need to:**
1. Deploy the blueprint
2. Configure environment variables (URL, email, optional S3)
3. Complete Ghost setup in admin panel
4. Start blogging! 🎉

---

**Ready to deploy?** Start with **Step 1** above! 🚀

Good luck with your Ghost blog! 📝✨

