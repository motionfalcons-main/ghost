# Fix: Database Connection Error on Render

## The Problem
```
Error: getaddrinfo ENOTFOUND dpg-d453mqmmcj7s73fsrff0-a
```

This means Ghost cannot connect to your PostgreSQL database because the hostname cannot be resolved.

## Quick Fix Steps

### 1. Check if Database Exists
Go to your Render Dashboard and check:
- Do you have a PostgreSQL database created?
- What is its name?
- Is it in the same region as your web service (Oregon)?

### 2. Option A: Delete and Redeploy with Blueprint (Cleanest)

If you haven't done any setup yet:

1. **Delete the current web service** (not the database if you want to keep data)
2. **Commit and push the updated render.yaml** from this repo
3. **Deploy using Blueprint again:**
   - Go to Render Dashboard
   - Click "New +" → "Blueprint"
   - Connect your GitHub repository
   - This will create BOTH the web service AND database together

### 3. Option B: Manually Configure Existing Database

If you already have a database with data:

1. **Go to your database in Render Dashboard**
2. **Click "Connect" → "Internal Connection"**
3. **Copy the connection details**

4. **Go to your Ghost web service → Environment**
5. **Add these environment variables:**

```bash
# Add or update these variables:
database__client=postgres
database__connection__host=dpg-d453mqmmcj7s73fsrff0-a.oregon-postgres.render.com
database__connection__port=5432
database__connection__user=ghost
database__connection__password=YOUR_DATABASE_PASSWORD
database__connection__database=ghost_production

# Also ensure you have:
url=https://your-actual-service-name.onrender.com
NODE_ENV=production
```

**Important:** Replace:
- `dpg-d453mqmmcj7s73fsrff0-a.oregon-postgres.render.com` with your actual database hostname
- `YOUR_DATABASE_PASSWORD` with your actual database password
- `your-actual-service-name.onrender.com` with your actual Render service URL

6. **Save and Redeploy**

### 4. Option C: Use DATABASE_URL (Simplest)

Render can automatically set database connection via a single environment variable:

1. **Go to your database in Render Dashboard**
2. **Copy the "Internal Database URL"**
3. **Go to your Ghost web service → Environment**
4. **Remove individual database__ variables if they exist**
5. **Add this ONE variable:**

```bash
database__connection__connectionString=postgresql://ghost:PASSWORD@dpg-xxxxx.oregon-postgres.render.com/ghost_production
```

Replace with your actual internal database URL.

6. **Add this variable:**

```bash
database__client=postgres
```

7. **Save and Redeploy**

## Verify the Fix

After redeploying, check your logs:
- You should see: `Ghost server started in X.XXs`
- You should NOT see: `Invalid database host` or `ENOTFOUND`

## Still Having Issues?

### Check These Common Problems:

1. **Database and Web Service in Different Regions**
   - Both must be in the same region (Oregon) for internal hostname to work
   - Check: Dashboard → Your Database → Settings → Region
   - Check: Dashboard → Your Web Service → Settings → Region

2. **Database Name Mismatch**
   - render.yaml references `ghost-db`
   - But your actual database might be named differently
   - Solution: Update render.yaml name OR use manual environment variables

3. **Missing url Variable**
   - Ghost REQUIRES the `url` environment variable
   - Set it to your actual Render URL: `https://your-service.onrender.com`
   - Update it later when you add a custom domain

4. **Database Not Created Yet**
   - If deploying via Blueprint, ensure the database is created first
   - Check Dashboard for both services

## Next Steps After Database Connects

1. **Set up your site URL** (update `url` environment variable)
2. **Configure email** (SendGrid, Mailgun, etc.)
3. **Set up S3 storage** (Render disk is ephemeral!)
4. **Visit /ghost** to complete admin setup

## Need More Help?

Check these files in your repository:
- `RENDER_DEPLOYMENT.md` - Full deployment guide
- `ENVIRONMENT_VARIABLES.md` - All configuration options
- `QUICK_START_RENDER.md` - Quick start guide

