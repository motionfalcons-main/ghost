# Environment Variables for Render Deployment

Configure these environment variables in your Render Dashboard → Your Service → Environment.

## Required Variables

### Site Configuration
```bash
NODE_ENV=production
url=https://your-site.onrender.com
```

### Database Configuration
*These are automatically configured when using Render's managed MySQL database*

```bash
database__client=mysql
database__connection__host=<from_render_database>
database__connection__port=<from_render_database>
database__connection__user=<from_render_database>
database__connection__password=<from_render_database>
database__connection__database=<from_render_database>
```

### Email Configuration
**Choose ONE email service provider:**

#### Option 1: SendGrid (Recommended)
```bash
mail__transport=SMTP
mail__options__service=SendGrid
mail__options__host=smtp.sendgrid.net
mail__options__port=587
mail__options__auth__user=apikey
mail__options__auth__pass=<YOUR_SENDGRID_API_KEY>
```

#### Option 2: Mailgun
```bash
mail__transport=SMTP
mail__options__host=smtp.mailgun.org
mail__options__port=587
mail__options__auth__user=<YOUR_MAILGUN_SMTP_LOGIN>
mail__options__auth__pass=<YOUR_MAILGUN_SMTP_PASSWORD>
```

#### Option 3: Gmail (Development Only)
```bash
mail__transport=SMTP
mail__options__service=Gmail
mail__options__auth__user=your-email@gmail.com
mail__options__auth__pass=<YOUR_APP_PASSWORD>
```

## Highly Recommended Variables

### File Storage (AWS S3)
**⚠️ IMPORTANT:** Render's disk storage is ephemeral. Files will be lost on redeploy without external storage!

```bash
storage__active=s3
storage__s3__accessKeyId=<YOUR_AWS_ACCESS_KEY_ID>
storage__s3__secretAccessKey=<YOUR_AWS_SECRET_ACCESS_KEY>
storage__s3__region=us-east-1
storage__s3__bucket=<your-ghost-bucket>
storage__s3__assetHost=https://<your-bucket>.s3.amazonaws.com
```

#### With CloudFront CDN
```bash
storage__s3__assetHost=https://<your-cloudfront-domain>.cloudfront.net
```

## Optional Variables

### Admin URL (if different from main site)
```bash
admin__url=https://admin.yoursite.com
```

### Privacy Settings
```bash
privacy__useUpdateCheck=false
privacy__useGravatar=true
privacy__useRpcPing=false
privacy__useStructuredData=true
```

### Database Performance
```bash
database__pool__min=2
database__pool__max=10
```

### Redis Caching (if using Redis)
```bash
cache__redis__host=<your-redis-host>
cache__redis__port=6379
cache__redis__password=<your-redis-password>
```

### Stripe (for paid memberships)
```bash
stripe__secret_key=sk_live_...
stripe__public_key=pk_live_...
```

### Logging
```bash
logging__level=info
logging__rotation__enabled=true
```

### Content Path (Render specific)
```bash
paths__contentPath=/opt/render/project/src/ghost/core/content
```

## How to Set Environment Variables in Render

1. Go to your Render Dashboard
2. Select your Ghost web service
3. Click "Environment" in the left sidebar
4. Click "Add Environment Variable"
5. Enter the key (e.g., `url`) and value (e.g., `https://yoursite.onrender.com`)
6. Click "Save Changes"
7. Your service will automatically redeploy with the new variables

## Environment Variable Hierarchy

Ghost configuration follows this priority order:
1. Environment variables (highest priority)
2. `config.production.json` file
3. Default values (lowest priority)

For Render deployments, we recommend using environment variables for all sensitive data and dynamic configuration.

## Testing Locally

To test your production configuration locally:

1. Create a `.env` file in the root (DO NOT commit this file)
2. Copy the variables from this guide
3. Run: `source .env && yarn render:start`

## Security Best Practices

- ✅ Never commit environment variables to git
- ✅ Use Render's "Secret" checkbox for sensitive values
- ✅ Rotate API keys and passwords regularly
- ✅ Use different credentials for staging and production
- ✅ Enable 2FA on all third-party services (AWS, SendGrid, etc.)

