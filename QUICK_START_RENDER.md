# 🚀 Quick Start: Deploy Ghost to Render

Get your Ghost blog live in ~15 minutes!

## Prerequisites
- ✅ GitHub account with this repository
- ✅ [Render account](https://dashboard.render.com/register) (free to start)
- ✅ Email SMTP credentials (SendGrid has a free tier)

## Step-by-Step Deployment

### 1️⃣ Prepare Your Email Service (5 minutes)

**SendGrid (Recommended - Free tier: 100 emails/day)**

1. Sign up at [SendGrid](https://signup.sendgrid.com/)
2. Verify your email
3. Go to Settings → API Keys
4. Create a new API Key with "Mail Send" permission
5. **Copy and save the API key** (you'll need it in step 3)

### 2️⃣ Deploy to Render (2 minutes)

1. Go to [Render Dashboard](https://dashboard.render.com/)
2. Click **"New +"** → **"Blueprint"**
3. Connect your GitHub account and select this repository
4. Render will detect `render.yaml` automatically
5. Click **"Apply"**

Render will create:
- ✅ A web service for Ghost
- ✅ A MySQL database
- ✅ Persistent disk storage (10GB)

### 3️⃣ Configure Environment Variables (5 minutes)

In Render Dashboard → Your Ghost Service → Environment:

**Required Variables:**

```bash
# Your site URL (update after deploy with your actual URL)
url = https://your-app-name.onrender.com

# SendGrid Email (use your API key from Step 1)
mail__transport = SMTP
mail__options__service = SendGrid
mail__options__host = smtp.sendgrid.net
mail__options__port = 587
mail__options__auth__user = apikey
mail__options__auth__pass = YOUR_SENDGRID_API_KEY_HERE
```

**Important:** Click "Save Changes" - this will trigger a redeploy.

### 4️⃣ Wait for Deployment (5-10 minutes)

Watch the build logs. You'll see:
- Dependencies installing
- Ghost building
- Database initializing
- Service starting

When you see **"Your Ghost site is now available on https://..."** - you're done! 🎉

### 5️⃣ Complete Setup (2 minutes)

1. Visit `https://your-app-name.onrender.com/ghost`
2. Create your admin account
3. Set your site title and description
4. Start blogging! ✍️

---

## ⚠️ Important: File Storage

**Default Setup:** Files are stored locally and **will be lost on redeploy**.

**For Production:** Set up AWS S3 storage (see [RENDER_DEPLOYMENT.md](./RENDER_DEPLOYMENT.md#option-b-aws-s3-recommended-for-production))

---

## 🎯 Quick Troubleshooting

### Build fails?
- Check Node.js version in logs (should be v22.13.1)
- Verify `render.yaml` is in repository root

### Can't send emails?
- Verify SendGrid API key is correct
- Check "secret" checkbox for the API key in Render
- Test with a simple password reset

### Site loads but looks broken?
- Wait 2-3 minutes for assets to fully build
- Clear browser cache
- Check build logs for errors

### Images don't save?
- This is expected with default local storage
- Set up S3 for persistent storage (see deployment guide)

---

## 📚 Next Steps

1. **Custom Domain**: [Add your domain](https://render.com/docs/custom-domains) in Render settings
2. **S3 Storage**: Set up [AWS S3](./RENDER_DEPLOYMENT.md#option-b-aws-s3-recommended-for-production) for images
3. **Theme**: Install a [custom theme](https://ghost.org/themes/)
4. **Optimize**: Enable [CloudFlare CDN](https://www.cloudflare.com/) for better performance

---

## 💰 Costs

- **Starter Plan**: ~$15/month
  - Web Service: $7/month
  - MySQL Database: $7/month
  - 10GB Disk: ~$1/month

- **Free Tier**: Available (but limited)
  - Perfect for testing before going live

---

## 🆘 Need Help?

- 📖 [Full Deployment Guide](./RENDER_DEPLOYMENT.md)
- ✅ [Deployment Checklist](./DEPLOYMENT_CHECKLIST.md)
- 🔧 [Environment Variables Guide](./ENVIRONMENT_VARIABLES.md)
- 💬 [Ghost Forum](https://forum.ghost.org/)
- 📘 [Render Support](https://render.com/docs)

---

## 🎉 Success!

Once deployed, your Ghost blog is:
- ✅ Running on production-grade infrastructure
- ✅ Automatically SSL secured
- ✅ Auto-scaling ready
- ✅ Backed up daily (database)
- ✅ Ready for your content!

**Happy blogging!** 🚀

