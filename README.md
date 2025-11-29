# 🌐 Set Up CloudFront & Test Your Deployment

This branch covers **configuring CloudFront** to serve your static frontend securely over HTTPS and then **testing the full end-to-end system** — CloudFront → S3 → API Gateway → Lambda → S3 memory.

Everything below follows your instructions exactly, including *image placement* and *100% width formatting*, with **no horizontal rule lines**.

## Step 1: Get Your S3 Website Endpoint

You must use the **S3 static website hosting URL**, not the bucket name.

1. In AWS Console, go to **S3**
2. Open your **frontend bucket**
3. Click the **Properties** tab
4. Scroll to **Static website hosting**
5. Copy the **Bucket website endpoint**, for example:

```
http://twin-frontend-xxx.s3-website-us-east-1.amazonaws.com
```

6. Save this URL — you will need it for CloudFront setup.

## Step 2: Create a CloudFront Distribution

1. In AWS Console, search for **CloudFront**
2. Click **Create distribution**

### Step 2.1 — Origin Settings

1. Distribution name: `twin-distribution`
2. Click **Next**
3. Under **Add origin**:

   * Origin type: **Other** (NOT Amazon S3)
   * Origin domain name: **Paste the S3 website endpoint WITHOUT** `http://`

     * Example:

       ```
       twin-frontend-xxx.s3-website-us-east-1.amazonaws.com
       ```
   * Origin protocol policy: **HTTP only**
     (critical — S3 website hosting does not support HTTPS)
   * Origin name: `s3-static-website` (or leave default)
   * Leave all other settings as default
4. Click **Add origin**

### Step 2.2 — Cache Behaviour

1. Path pattern: `Default (*)`
2. Origin: select the one you just created
3. Viewer protocol policy: **Redirect HTTP to HTTPS**
4. Allowed methods: **GET, HEAD**
5. Cache policy: **CachingOptimized**
6. Click **Next**

### Step 2.3 — WAF Settings

* Select **Do not enable security protections** (avoids ~$14/month)
* Click **Next**

### Step 2.4 — Final Settings

1. Price class: **Use only North America and Europe** (to reduce cost)
2. Default root object: `index.html`
3. Click **Next**

### Step 2.5 — Create the Distribution

* Review settings
* Click **Create distribution**

Deployment usually takes **5–15 minutes**. Status will change from *Deploying* to *Enabled*.

## Step 3: Update Lambda CORS Settings (Important!)

You now need to restrict CORS so that only **CloudFront** is allowed to call your API.

1. Go to **Lambda → twin-api → Configuration → Environment variables**
2. Get your CloudFront URL:

   * Go to **CloudFront → Your distribution**
   * Copy the **Distribution domain name**

     * Example:

       ```
       d1234abcd.cloudfront.net
       ```
3. Update the environment variable:

```
CORS_ORIGINS = https://YOUR-CLOUDFRONT-DOMAIN.cloudfront.net
```

**This MUST be exact:**

* Must start with `https://`
* Must **not** end with `/`
* Must exactly match the value in CloudFront

If this value is incorrect, you will lose at least **2–6 hours** debugging confusing CORS errors.
Take your time and get it right.

<img src="img/aws_lambda/cloudfront_distribution.png" width="100%">

4. Click **Save**

## Step 4: Invalidate CloudFront Cache

After updating CORS, CloudFront must be invalidated so the new settings apply.

1. Go to **CloudFront → Your distribution**
2. Click **Invalidations**
3. Click **Create invalidation**
4. Enter:

```
/*
```

5. Click **Create invalidation**

## Step 5: Test Your Deployment

Now the fun part — verify everything works end-to-end.

## Step 5.1 — Access Your Digital Twin

1. Open your CloudFront URL:

```
https://YOUR-DISTRIBUTION.cloudfront.net
```

2. Your Digital Twin frontend should load instantly over HTTPS
3. Test the chat interaction
4. Ensure responses are fast and memory works

## Step 5.2 — Verify Memory Storage in S3

1. Go to **S3 → Your memory bucket**
2. You should now see JSON files such as:

```
session-123abc.json
session-456xyz.json
```

These represent **persistent conversation history**, meaning memory continues even when Lambda shuts down.

## Step 5.3 — Check Lambda Logs in CloudWatch

1. In AWS Console, go to **CloudWatch**
2. Open **Log groups**
3. Select:

```
/aws/lambda/twin-api
```

4. Inspect logs for:

   * incoming requests
   * errors
   * JSON memory writes
   * latency information

This is your main debugging tool for production.