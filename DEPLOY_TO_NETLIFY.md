# Deploying to Netlify with wlong.io Domain

## Quick Deploy (Drag & Drop)

1. Go to https://app.netlify.com and sign up/login
2. Drag your `public/` folder to the Netlify deploy area
3. Once deployed, go to **Site settings** → **Domain management**
4. Click **Add custom domain** → Enter `wlong.io`
5. Follow Netlify's DNS instructions to update your domain's DNS records

## Deploy via GitHub (Recommended for Auto-Updates)

### If you haven't deployed yet:

1. Push your code to GitHub (already done)
2. Go to https://app.netlify.com
3. Click **Add new site** → **Import an existing project**
4. Connect to GitHub (authorize Netlify if needed)
5. Select `personal-website` repository
6. Build settings:
   - **Build command**: `npm run build --legacy-peer-deps`
   - **Publish directory**: `public`
   - **Base directory**: (leave blank)
7. Click **Deploy site**
8. Add custom domain `wlong.io` in site settings
9. Update DNS records at your domain registrar

### If you already deployed via drag-and-drop and want to connect GitHub:

1. In your Netlify dashboard, go to your site
2. Click **Site settings** → **Build & deploy** → **Continuous Deployment**
3. Click **Link site to Git**
4. Connect to GitHub (authorize Netlify if needed)
5. Select `personal-website` repository
6. Build settings:
   - **Build command**: `npm run build --legacy-peer-deps`
   - **Publish directory**: `public`
   - **Base directory**: (leave blank)
   - **Branch to deploy**: `main`
7. Click **Save**
8. Netlify will automatically trigger a new build from your GitHub repo

### Automatic Deploys

Once connected, Netlify will automatically:

- Build and deploy when you push to `main` branch
- Show deploy previews for pull requests
- Keep your site in sync with your GitHub repository

## DNS Configuration

### For Squarespace-managed domains:

Add an **ALIAS record** in Squarespace DNS settings:

- **Host**: `@` (or leave blank for apex domain)
- **TTL**: `3600` (1 hour)
- **Data/Target**: `apex-loadbalancer.netlify.com`

Optional: Add a **CNAME record** for www subdomain:

- **Host**: `www`
- **TTL**: `3600`
- **Data/Target**: `wlong.io`

## HTTPS Configuration

Netlify automatically provides **free SSL certificates** via Let's Encrypt. Here's how to ensure HTTPS works:

### Step 1: Add Domain to Netlify

1. Go to **Site settings** → **Domain management**
2. Click **Add custom domain** → Enter `wlong.io`
3. Netlify will automatically start provisioning an SSL certificate

### Step 2: Verify DNS Configuration

1. In Netlify, go to **Domain settings** → Check DNS status
2. It should show "DNS configured correctly" once DNS propagates
3. SSL certificate provisioning usually takes **5-60 minutes** after DNS is correct

### Step 3: Enable HTTPS

1. In **Domain settings**, ensure **HTTPS** is enabled (it's automatic)
2. Enable **Force HTTPS** (redirects HTTP to HTTPS automatically)
3. This is also configured in `netlify.toml` with redirect rules

### Troubleshooting HTTPS

**If HTTPS doesn't work yet:**

1. **Wait 5-60 minutes** after DNS is configured - certificate provisioning takes time
2. **Check DNS propagation**: Use `dig wlong.io` or visit https://dnschecker.org
3. **Verify in Netlify dashboard**: Domain settings should show certificate status
4. **Clear browser cache**: Sometimes browsers cache SSL errors
5. **Check certificate status**: Netlify dashboard → Domain settings → SSL certificate

**Common Issues:**

- DNS not fully propagated (can take up to 48 hours, usually much faster)
- Certificate still provisioning (wait 5-60 minutes)
- Browser cache (clear cache or use incognito mode)

## Alternative: Use a Subdomain

If you want to keep Squarespace for some pages:

- Host Gatsby on `www.wlong.io` or `site.wlong.io`
- Keep Squarespace on `wlong.io` and redirect to the Gatsby site
