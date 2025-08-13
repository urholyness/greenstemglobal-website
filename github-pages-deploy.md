# GitHub Pages Deployment (Quick & Free)

## Why GitHub Pages for Now?
- **Free hosting** for static sites
- **Instant deployment** (under 5 minutes)
- **HTTPS included** automatically
- **Perfect for testing** before AWS production
- **No server management** required

## Step 1: Create GitHub Repository
```bash
# Initialize git if not already done
git init
git add .
git commit -m "Initial commit"

# Create repository on GitHub (via web or CLI)
# Name it: greenstemglobal-website
```

## Step 2: Push to GitHub
```bash
# Add your GitHub repository as origin
git remote add origin https://github.com/YOUR_USERNAME/greenstemglobal-website.git

# Push to main branch
git branch -M main
git push -u origin main
```

## Step 3: Enable GitHub Pages
1. Go to your repository on GitHub
2. Click **Settings** → **Pages** (left sidebar)
3. Under "Source", select **Deploy from a branch**
4. Select **main** branch and **/ (root)** folder
5. Click **Save**

## Step 4: Access Your Site
Your site will be available at:
```
https://YOUR_USERNAME.github.io/greenstemglobal-website/
```

Site usually goes live in 2-10 minutes.

## Step 5: Custom Domain (Optional)
1. In repository Settings → Pages
2. Add custom domain: `greenstemglobal.com`
3. At your domain registrar, add:
   ```
   Type: CNAME
   Name: www
   Value: YOUR_USERNAME.github.io
   
   Type: A
   Name: @
   Value: 185.199.108.153
          185.199.109.153
          185.199.110.153
          185.199.111.153
   ```

## Files Required
- `index.html` - Main website (✅ already created)
- `img/` folder with:
  - `our_logo.png` ✅
  - `chilies.jpg` ⚠️ (you need to add)
  - `french_beans.jpg` ⚠️ (you need to add)
  - `passion_fruit.jpg` ⚠️ (you need to add)
  - `mangoes.jpg` ⚠️ (you need to add)

## Comparison: GitHub Pages vs AWS

### GitHub Pages (Development/Testing)
**Pros:**
- ✅ Free hosting
- ✅ Automatic HTTPS
- ✅ Git-based deployment
- ✅ Zero configuration
- ✅ Perfect for static sites

**Cons:**
- ❌ No server-side processing
- ❌ 100GB bandwidth limit/month
- ❌ No advanced CDN features
- ❌ Limited to static content

### AWS S3 + CloudFront (Production)
**Pros:**
- ✅ Unlimited scalability
- ✅ Global CDN with 400+ edge locations
- ✅ Advanced caching controls
- ✅ Can add Lambda@Edge for dynamic content
- ✅ Ready for future API integration
- ✅ Custom error pages
- ✅ Detailed analytics

**Cons:**
- ❌ Costs ~$5-15/month
- ❌ More complex setup
- ❌ Requires AWS knowledge

## Recommendation
1. **Now (Testing):** Use GitHub Pages for immediate deployment
2. **December 2025 (Production):** Migrate to AWS when adding:
   - Real-time data from farms
   - API integration for live metrics
   - User authentication
   - Database connectivity
   - Advanced analytics

## Quick Deploy Commands
```bash
# After adding your product images
git add img/
git commit -m "Add product images"
git push

# Site auto-updates on GitHub Pages
```

Your site is ready for GitHub Pages deployment! 🚀