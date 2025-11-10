# 🔐 Render Environment Variables - Complete Reference

This guide explains every environment variable you might need for deploying Ghost on Render.

---

## 🚨 Critical Variables (Must Configure)

These MUST be set for Ghost to work:

### 1. Site URL
```bash
url = https://your-service.onrender.com
```
**What it does**: Tells Ghost where it's hosted  
**How to get**: After creating the service, find it in Render Dashboard → Service → URL  
**Example**: `https://ghost-abc123.onrender.com`  
**⚠️ Critical**: Must match your actual domain (or Render subdomain)  
**When to update**: After getting custom domain, update this and redeploy

---

### 2. Email Configuration

Ghost needs email to:
- Send password reset links
- Invite team members
- Send member newsletters
- Notify about comments

#### Mail Transport
```bash
mail__transport = SMTP
```
**What it does**: Tells Ghost to use SMTP for email  
**Value**: Always `SMTP` (unless using Direct/Sendmail locally)

#### Email Service (Choose ONE option)

**Option A: SendGrid (Recommended)** ⭐
```bash
mail__options__service = SendGrid
mail__options__host = smtp.sendgrid.net
mail__options__port = 587
mail__options__auth__user = apikey
mail__options__auth__pass = SG.xxxxxxxxxxxxx
```
**Why**: Free tier (100 emails/day), reliable, easy setup  
**Get API Key**: https://app.sendgrid.com/settings/api_keys  
**⚠️ Important**: `auth__user` is literally the word `apikey`

**Option B: Mailgun**
```bash
mail__options__service = Mailgun
mail__options__host = smtp.mailgun.org
mail__options__port = 587
mail__options__auth__user = postmaster@your-domain.mailgun.org
mail__options__auth__pass = your-mailgun-password
```
**Get Credentials**: https://app.mailgun.com/app/sending/domains

**Option C: Gmail (Testing Only)** ⚠️
```bash
mail__options__service = Gmail
mail__options__host = smtp.gmail.com
mail__options__port = 587
mail__options__auth__user = your-email@gmail.com
mail__options__auth__pass = your-app-password
```
**Note**: Not recommended for production (daily limits, security)  
**Get App Password**: https://myaccount.google.com/apppasswords (requires 2FA)

**Option D: Generic SMTP**
```bash
mail__options__host = smtp.yourmailserver.com
mail__options__port = 587
mail__options__secure = false
mail__options__auth__user = your-username
mail__options__auth__pass = your-password
```
**Use for**: Any SMTP server not listed above

---

## 🗄️ Database Variables (Auto-Configured)

These are automatically set by Render when using the Blueprint:

```bash
database__client = postgres
database__connection__host = [auto from render]
database__connection__port = [auto from render]
database__connection__user = [auto from render]
database__connection__password = [auto from render]
database__connection__database = [auto from render]
```

**You don't need to set these manually!**

If using external database:
```bash
database__connection__host = your-db-host.com
database__connection__port = 5432
database__connection__user = your-db-user
database__connection__password = your-db-password
database__connection__database = ghost_production
database__connection__ssl = true
database__connection__ssl__rejectUnauthorized = false
```

---

## 💾 Storage Configuration

### Local Storage (Default - Not Recommended for Production)
```bash
storage__active = local
```
**⚠️ WARNING**: Files uploaded to local storage will be **LOST on every redeploy**!  
**Use case**: Development/testing only

### AWS S3 Storage (Recommended for Production) ⭐
```bash
storage__active = s3
storage__s3__accessKeyId = AKIAIOSFODNN7EXAMPLE
storage__s3__secretAccessKey = wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
storage__s3__region = us-west-2
storage__s3__bucket = ghost-your-site
storage__s3__assetHost = https://ghost-your-site.s3.amazonaws.com

# Optional: Force path style (for DigitalOcean Spaces, etc.)
storage__s3__forcePathStyle = true

# Optional: Custom endpoint (for non-AWS S3-compatible services)
storage__s3__endpoint = https://nyc3.digitaloceanspaces.com
```

**With CloudFront CDN**:
```bash
storage__s3__assetHost = https://d1234567890.cloudfront.net
```

**⚠️ Security**: Mark `accessKeyId` and `secretAccessKey` as SECRET in Render!

**S3 Setup Steps**:
1. Create S3 bucket
2. Configure CORS (see deployment guide)
3. Create IAM user with S3 permissions
4. Get access keys
5. Add to Render environment variables

### DigitalOcean Spaces
```bash
storage__active = s3
storage__s3__accessKeyId = YOUR_DO_ACCESS_KEY
storage__s3__secretAccessKey = YOUR_DO_SECRET_KEY
storage__s3__region = nyc3
storage__s3__bucket = your-space-name
storage__s3__endpoint = https://nyc3.digitaloceanspaces.com
storage__s3__forcePathStyle = true
storage__s3__assetHost = https://your-space-name.nyc3.digitaloceanspaces.com
```

### Cloudflare R2
```bash
storage__active = s3
storage__s3__accessKeyId = YOUR_R2_ACCESS_KEY
storage__s3__secretAccessKey = YOUR_R2_SECRET_KEY
storage__s3__region = auto
storage__s3__bucket = your-bucket-name
storage__s3__endpoint = https://YOUR_ACCOUNT_ID.r2.cloudflarestorage.com
storage__s3__forcePathStyle = true
storage__s3__assetHost = https://your-custom-domain.com
```

---

## ⚙️ Server Configuration (Pre-Configured)

These are already set in `render.yaml`:

```bash
NODE_ENV = production
PORT = 10000
server__host = 0.0.0.0
server__port = 10000
```

**Don't change these unless you know what you're doing!**

---

## 🔒 Optional Security Variables

### Disable Update Checks
```bash
privacy__useUpdateCheck = false
```
**What it does**: Disables Ghost phoning home for update checks  
**Recommended**: `false` for self-hosted

### Admin URL (Advanced)
```bash
admin__url = https://admin.yourdomain.com
```
**Use case**: Separate admin interface from public site  
**Example**: Public site at `blog.com`, admin at `admin.blog.com`  
**Note**: Requires DNS setup and reverse proxy configuration

---

## 🚀 Performance Variables

### Database Connection Pool
```bash
database__pool__min = 2
database__pool__max = 10
database__pool__acquireTimeoutMillis = 60000
database__pool__createTimeoutMillis = 30000
database__pool__destroyTimeoutMillis = 5000
database__pool__idleTimeoutMillis = 30000
database__pool__reapIntervalMillis = 1000
database__pool__createRetryIntervalMillis = 200
```
**When to use**: High-traffic sites (1000+ visitors/day)  
**Default values work for most sites**

### Logging Level
```bash
logging__level = info
```
**Options**: `error`, `warn`, `info`, `debug`  
**Production**: `info` or `warn`  
**Debugging**: `debug`

---

## 📧 Advanced Email Configuration

### Email From Address
```bash
mail__from = "Ghost Blog <noreply@yourdomain.com>"
```
**Default**: Uses site title  
**Custom**: Set your preferred sender name/email

### SMTP Connection Settings
```bash
mail__options__connectionTimeout = 10000
mail__options__greetingTimeout = 10000
mail__options__socketTimeout = 10000
```
**Use case**: Slow SMTP servers or high latency

### TLS/SSL Configuration
```bash
mail__options__secure = true
mail__options__requireTLS = true
mail__options__tls__rejectUnauthorized = false
```
**secure = true**: Use implicit TLS (port 465)  
**secure = false**: Use STARTTLS (port 587)

---

## 🌐 URL Configuration (Advanced)

### Separate Admin URL
```bash
url = https://blog.yourdomain.com
admin__url = https://admin.yourdomain.com
```

### Using Subdirectory
```bash
url = https://yourdomain.com/blog
```
**Note**: Requires reverse proxy configuration

---

## 💳 Stripe Integration (Memberships)

For paid memberships/subscriptions:

```bash
# Test Mode
stripe__publishableKey = pk_test_xxxxxxxxxxxxx
stripe__secretKey = sk_test_xxxxxxxxxxxxx

# Production Mode
stripe__publishableKey = pk_live_xxxxxxxxxxxxx
stripe__secretKey = sk_live_xxxxxxxxxxxxx
```

**Get Keys**: https://dashboard.stripe.com/apikeys  
**⚠️ Security**: Mark secret key as SECRET in Render!

### Stripe Webhook
```bash
stripe__webhook__secret = whsec_xxxxxxxxxxxxx
```
**Setup**:
1. In Stripe Dashboard → Webhooks
2. Add endpoint: `https://your-site.com/members/webhooks/stripe`
3. Copy webhook secret
4. Add to environment variables

---

## 📊 Analytics & Monitoring

### Sentry Error Tracking
```bash
sentry__dsn = https://xxxxx@sentry.io/xxxxx
sentry__environment = production
```

### Google Analytics
Configure in Ghost Admin → Settings → Integrations

---

## 🔑 Security Tokens (Auto-Generated)

These are automatically generated by Ghost on first run:

```bash
# DO NOT SET MANUALLY - Ghost generates these
database__knexMigratorFilePath
paths__contentPath
```

---

## ✅ Quick Reference: Minimum Required Variables

For basic Ghost deployment on Render:

```bash
# MUST SET:
url = https://your-service.onrender.com

# Email (choose one service):
mail__transport = SMTP
mail__options__service = SendGrid
mail__options__host = smtp.sendgrid.net
mail__options__port = 587
mail__options__auth__user = apikey
mail__options__auth__pass = YOUR_SENDGRID_API_KEY

# Storage (recommended: S3)
storage__active = s3
storage__s3__accessKeyId = YOUR_AWS_KEY
storage__s3__secretAccessKey = YOUR_AWS_SECRET
storage__s3__region = us-west-2
storage__s3__bucket = your-bucket
storage__s3__assetHost = https://your-bucket.s3.amazonaws.com

# AUTO-CONFIGURED (via render.yaml):
NODE_ENV = production
PORT = 10000
server__host = 0.0.0.0
server__port = 10000
database__client = postgres
database__connection__* = [from Render managed database]
```

---

## 🎯 How to Set Variables in Render

1. Go to Render Dashboard
2. Select your Ghost service
3. Click **Environment** in left sidebar
4. Click **Add Environment Variable**
5. Enter Key and Value
6. For sensitive values: Check **Secret** ✅
7. Click **Save Changes** (triggers redeploy)

---

## 🐛 Debugging Environment Variables

### Check Current Variables
In Render logs, Ghost prints sanitized config on startup (secrets are hidden)

### Common Issues

**"Database connection failed"**
```
Check: database__connection__* variables
Ensure: Database service is running
```

**"Cannot send email"**
```
Check: mail__options__auth__pass is correct
Ensure: Mark as SECRET in Render
Test: SMTP credentials with external tool
```

**"Images don't load after redeploy"**
```
Check: storage__active = s3 (not local)
Ensure: S3 credentials are correct
```

**"Site loads but shows wrong URL"**
```
Check: url variable matches actual URL
Update: url after adding custom domain
Redeploy: After changing url
```

---

## 📋 Environment Variables Checklist

Before going live:

- [ ] `url` set to actual domain
- [ ] Email service configured and tested
- [ ] S3 storage configured (if production)
- [ ] All secrets marked as SECRET in Render
- [ ] Database variables auto-configured
- [ ] Site loads at configured URL
- [ ] Test email sends successfully
- [ ] Images upload and persist after redeploy

---

## 📚 Additional Resources

- [Ghost Configuration Docs](https://ghost.org/docs/config/)
- [Render Environment Variables](https://render.com/docs/environment-variables)
- [Ghost + S3 Setup](https://ghost.org/integrations/amazon-s3/)

---

## 🆘 Still Need Help?

- Read: [FRESH_RENDER_DEPLOYMENT.md](./FRESH_RENDER_DEPLOYMENT.md)
- Check: [PRE_FLIGHT_CHECKLIST.md](./PRE_FLIGHT_CHECKLIST.md)
- Forum: [forum.ghost.org](https://forum.ghost.org/)
- Docs: [ghost.org/docs](https://ghost.org/docs/)

---

**Remember**: Environment variables use double underscores `__` for nesting!

Example: `mail__options__auth__pass` = `mail.options.auth.pass` in Ghost config

