# N8N Workflow Setup Guide - DRMSI Auto Publisher

## 🎯 What This Workflow Does

**Automatically every 4 hours:**
1. Selects 4 different assets (rotates based on time)
2. Fetches live price data from Yahoo Finance (FREE, no API key)
3. Generates professional analysis using OpenAI/Gemini
4. Creates HTML article card
5. Pushes standalone article to `articles/symbol-date.html`
6. Updates homepage `index.html` with latest 6 articles
7. Sends Telegram notification (optional)

**Result:** 4-6 fresh articles daily, fully automated!

---

## 📋 Prerequisites

### 1. GitHub Personal Access Token

**Create GitHub Token:**
1. Go to: https://github.com/settings/tokens
2. Click "Generate new token (classic)"
3. Name: `n8n-drmsi-publisher`
4. Select scopes:
   - ✅ `repo` (Full control of private repositories)
   - ✅ `workflow` (Update GitHub Action workflows)
5. Click "Generate token"
6. **Copy token immediately** (you won't see it again!)

**Save as:** `ghp_xxxxxxxxxxxxxxxxxxxx`

### 2. OpenAI or Gemini API Key

**Option A: OpenAI (Recommended)**
1. Go to: https://platform.openai.com/api-keys
2. Create new secret key
3. **Copy key:** `sk-proj-xxxxxxxxxxxxxxxx`
4. **Cost:** ~$0.50-1 per day (GPT-3.5-turbo)

**Option B: Gemini (Free tier available)**
1. Go to: https://makersuite.google.com/app/apikey
2. Create API key
3. **Copy key:** `AIzaSyxxxxxxxxxxxxxxxxx`
4. **Cost:** Free tier: 60 requests/minute

### 3. Telegram Bot (Optional - for notifications)

1. Open Telegram, search for `@BotFather`
2. Send: `/newbot`
3. Follow prompts, get token: `123456:ABC-DEF...`
4. Get your Chat ID:
   - Search `@userinfobot`
   - Send `/start`
   - Copy your ID: `123456789`

---

## 🚀 Step-by-Step Setup

### Step 1: Prepare GitHub Repository

```bash
# Create articles folder
cd drmsi.com
mkdir articles
echo "# Articles" > articles/README.md

# Commit
git add articles/
git commit -m "Add articles folder for automated posts"
git push origin main
```

### Step 2: Update index.html (Add data-date attribute)

Your current article cards need one small addition:

```html
<!-- Change from: -->
<article class="article-card">

<!-- To: -->
<article class="article-card" data-date="2025-10-01T10:00:00Z">
```

I'll provide the updated index.html after you confirm.

### Step 3: Install n8n (if not already)

**Option A: n8n Cloud (Easiest)**
1. Go to: https://n8n.io
2. Sign up for free
3. Create new workflow
4. Skip to Step 4

**Option B: Self-hosted (Docker)**
```bash
docker run -d \
  --name n8n \
  -p 5678:5678 \
  -v ~/.n8n:/home/node/.n8n \
  n8nio/n8n

# Access: http://localhost:5678
```

**Option C: Self-hosted (npm)**
```bash
npm install n8n -g
n8n start

# Access: http://localhost:5678
```

### Step 4: Import Workflow

1. Open n8n interface
2. Click "+" (new workflow)
3. Click "..." menu → "Import from File"
4. Upload the JSON file I created
5. Or copy-paste the JSON directly

### Step 5: Configure Credentials

#### A. GitHub Credentials

1. In n8n, click "Credentials" tab
2. Click "+ Add Credential"
3. Select "GitHub OAuth2"
4. Fill in:
   - **Access Token:** `ghp_your_token_here`
5. Test connection
6. Save

#### B. OpenAI/Gemini Credentials

**For OpenAI:**
1. Add Credential → "OpenAI"
2. API Key: `sk-proj-your_key_here`
3. Save

**For Gemini:**
1. Add Credential → "Google PaLM/Gemini"
2. API Key: `AIzaSy_your_key_here`
3. Save

### Step 6: Set Environment Variables

In n8n workflow settings:

```javascript
// Click "Settings" → "Environment Variables"

GITHUB_OWNER=drmsi
GITHUB_REPO=drmsi.com

// Optional (for Telegram notifications)
TELEGRAM_BOT_TOKEN=123456:ABC-DEF...
TELEGRAM_CHAT_ID=123456789
```

### Step 7: Activate Workflow

1. Review all nodes (green = configured)
2. Click "Active" toggle (top right)
3. Workflow will run every 4 hours

### Step 8: Test Manually

1. Click "Execute Workflow" button
2. Watch execution in real-time
3. Check for errors
4. Verify:
   - Article created in `articles/` folder
   - Homepage updated with new article card

---

## 🎨 Workflow Customization

### Change Schedule (Cron Expression)

```javascript
// Every 4 hours (current)
0 */4 * * *

// Every 6 hours
0 */6 * * *

// Daily at 8 AM
0 8 * * *

// Twice daily (8 AM, 8 PM)
0 8,20 * * *

// Every 2 hours
0 */2 * * *
```

### Add More Symbols

Edit "Smart Symbol Selection" node:

```javascript
const allSymbols = [
  // Add your symbols here
  'DOGEUSD',  // Dogecoin
  'ADAUSD',   // Cardano
  'DOTUSD',   // Polkadot
  'NVDA',     // NVIDIA
  'META',     // Meta
  'NFLX'      // Netflix
];
```

### Change Article Count

Edit "Update index.html" node:

```javascript
// Change from 6 to your desired number
const latestArticles = existingArticles.slice(0, 9); // Show 9 articles
```

### Adjust AI Prompt

Edit "Generate Article with AI" node to change:
- Tone (professional, casual, technical)
- Length (short, medium, long)
- Format (more/less sections)
- Focus (technical, fundamental, both)

---

## 🔍 Monitoring & Troubleshooting

### Check Workflow Execution

1. n8n Dashboard → "Executions"
2. View success/failure
3. Click execution to see details

### Common Issues

#### Issue 1: GitHub Authentication Failed

```
Error: Bad credentials
```

**Fix:**
- Regenerate GitHub token
- Make sure `repo` scope is selected
- Update credentials in n8n

#### Issue 2: Yahoo Finance Returns No Data

```
Error: Cannot read property 'meta' of undefined
```

**Fix:**
- Symbol format might be wrong
- Use correct format:
  - Forex: `EURUSD=X`
  - Crypto: `BTC-USD`
  - Stocks: `AAPL`
- Update "Fetch Price Data" node

#### Issue 3: AI Generation Fails

```
Error: Insufficient quota / Rate limit
```

**Fix:**
- Check API credits
- Reduce frequency
- Switch to Gemini (has free tier)

#### Issue 4: index.html Not Updating

```
Error: Could not find articles grid
```

**Fix:**
- Make sure index.html has correct structure
- Check `<div class="articles-grid" id="articlesGrid">`
- Add `data-date` attribute to article cards

---

## 📊 Expected Costs

### Free Tier (Recommended for Testing)

- **n8n Cloud:** Free (5,000 workflow executions/month)
- **GitHub:** Free
- **Yahoo Finance:** Free (no API key needed)
- **Gemini AI:** Free (60 requests/min)
- **Total:** $0/month ✅

### Paid Tier (For Scale)

- **n8n Cloud Pro:** $20/month
- **OpenAI GPT-3.5:** ~$0.50/day = $15/month
- **Total:** ~$35/month

**Runs per month:** 4 articles × 6 times/day = 24 articles/day = 720/month

---

##