# AWS S3 + CloudFront Deployment Instructions

## Prerequisites
1. AWS Account with appropriate permissions
2. AWS CLI installed and configured

## Step 1: Create S3 Bucket
```bash
# Replace 'your-bucket-name' with your desired bucket name
aws s3 mb s3://greenstemglobal-website --region us-east-1
```

## Step 2: Configure S3 for Static Website Hosting
```bash
aws s3 website s3://greenstemglobal-website --index-document home10.html --error-document home10.html
```

## Step 3: Upload Website Files
```bash
# Upload all files to S3
aws s3 sync . s3://greenstemglobal-website --exclude "*.md" --exclude ".git/*" --exclude "awscliv2.zip" --exclude "aws/*"

# Set proper content types
aws s3 cp home10.html s3://greenstemglobal-website/ --content-type "text/html"
aws s3 cp s3://greenstemglobal-website/img/ s3://greenstemglobal-website/img/ --recursive --content-type "image/png"
```

## Step 4: Make Bucket Public
Create bucket policy (replace bucket name):
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadGetObject",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::greenstemglobal-website/*"
    }
  ]
}
```

Apply policy:
```bash
aws s3api put-bucket-policy --bucket greenstemglobal-website --policy file://bucket-policy.json
```

## Step 5: Create CloudFront Distribution
```bash
# Create distribution configuration
aws cloudfront create-distribution --distribution-config file://cloudfront-config.json
```

## Step 6: Custom Domain Setup (Using Your Existing Domain)

### Option A: If your domain is with Route 53
```bash
# Request SSL certificate
aws acm request-certificate \
  --domain-name greenstemglobal.com \
  --subject-alternative-names "*.greenstemglobal.com" \
  --validation-method DNS \
  --region us-east-1
```

### Option B: If your domain is elsewhere (GoDaddy, Namecheap, etc)

1. **Create SSL Certificate in AWS Certificate Manager:**
   - Go to AWS Certificate Manager (us-east-1 region)
   - Request a public certificate
   - Add: `greenstemglobal.com` and `www.greenstemglobal.com`
   - Choose DNS validation
   - Copy the CNAME records provided

2. **Add Validation Records at Your Registrar:**
   - Login to your domain registrar
   - Add the CNAME records from ACM
   - Wait 5-30 minutes for validation

3. **Update CloudFront Distribution:**
   - Add alternate domain names: `greenstemglobal.com`, `www.greenstemglobal.com`
   - Select your SSL certificate
   - Save changes

4. **Point Your Domain to CloudFront:**
   ```
   Type: CNAME
   Name: www
   Value: [your-distribution].cloudfront.net
   
   Type: A/ALIAS (or use Cloudflare for root domain)
   Name: @ 
   Value: [your-distribution].cloudfront.net
   ```

5. **Wait for DNS Propagation:** 5-48 hours

## Alternative: Quick Manual Setup
1. Go to AWS S3 Console
2. Create bucket: `greenstemglobal-website`
3. Upload files:
   - `home10.html` (main website)
   - `img/our_logo.png` (logo - already in folder)
   - `img/chilies.jpg` (add this)
   - `img/french_beans.jpg` (add this)
   - `img/passion_fruit.jpg` (add this)
   - `img/mangoes.jpg` (add this)
4. Enable static website hosting
5. Set bucket to public read
6. Create CloudFront distribution
7. Follow custom domain setup above

## Cost Estimate
- S3 hosting: ~$1-5/month
- CloudFront: ~$1-10/month (depending on traffic)
- SSL Certificate: Free (AWS Certificate Manager)
- Your existing domain: No additional cost

## Access URLs
- Development: http://localhost:8080/home10.html
- S3: http://greenstemglobal-website.s3-website-us-east-1.amazonaws.com
- CloudFront: https://[distribution-id].cloudfront.net
- **Your Domain: https://greenstemglobal.com** (after DNS setup)