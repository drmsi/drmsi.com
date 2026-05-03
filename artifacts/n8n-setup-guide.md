# DRMSI Enhanced N8N Workflow - Setup Guide

## 🎯 What This Workflow Does

This enhanced n8n workflow automatically:
1. **Selects 4 symbols** every 4 hours based on time-based rotation
2. **Fetches real-time data** from Yahoo Finance (correct format: `EURUSD=X`, `BTC-USD`, etc.)
3. **Generates SEO-optimized articles** using OpenAI with proper HTML structure
4. **Creates two versions**: Full article page + Short card for homepage
5. **Pushes to GitHub**: Standalone article + updates index.html
6. **Includes TradingView charts** on each article page
7. **Notifies via Telegram** (optional)

---

## 📋 Key Features Implemented

### ✅ Symbol Format Mapping
- **Forex**: `EURUSD=X` (Yahoo) → `FX:EURUSD` (TradingView)
- **Crypto**: `BTC-USD` (Yahoo) → `BINANCE:BTCUSDT` (TradingView)
- **Stocks**: `AAPL` (Yahoo) → `NASDAQ:AAPL` (TradingView)
- **Commodities**: `GC=F` (Gold futures) → `OANDA:XAUUSD` (TradingView)

### ✅ SEO Optimization
- **Meta descriptions** (150-160 chars)
- **Open Graph tags** for social sharing
- **Twitter Card** metadata
- **JSON-LD structured data** (Article schema)
- **Canonical URLs**
- **Semantic HTML5** tags

### ✅ Article Structure
- **Short version**: 150-char summary + metadata on index.html
- **Full version**: Complete article with sections in `articles/slug.html`
- **Read More button**: Links to full article
- **TradingView chart**: Embedded with dark theme, 4H interval

### ✅ GitHub Integration
- **Smart injection**: Prepends new articles to index.html
- **Keeps 6 articles**: Removes oldest when adding new
- **Two file operations**:
  1. Create `articles/[slug].html`
  2. Update `index.html` (read → modify → push)

### ✅ Error Handling
- **Retry logic**: Yahoo Finance (3 retries, 2s delay)
- **Continue on fail**: Telegram notification won't stop workflow
- **Validation**: Checks Yahoo Finance response structure

---

## 🔧 Setup Instructions

### Step 1: Environment Variables

In your n8n instance, set these environment variables:

```bash
GITHUB_OWNER=drmsi
GITHUB_REPO=drmsi.com
TELEGRAM_BOT_TOKEN=your_bot_token  # Optional
TELEGRAM_CHAT_ID=your_chat_id      # Optional
```

**How to set in n8n Cloud:**
1. Go to Settings → Environment Variables
2. Add each variable
3. Save

**How to set in self-hosted n8n:**
Add to `.env` file or set in Docker/system environment.

---

### Step 2: Credentials Setup

You need to create 3 credentials in n8n:

#### A) OpenAI API Credential

1. In n8n, go to **Credentials** → **New**
2. Search for "OpenAI"
3. Name it: `OpenAI account`
4. Add your API key: `sk-proj-xxxxx`
5. Save

**Get OpenAI API Key:**
- Go to https://platform.openai.com/api-keys
- Click "Create new secret key"
- Copy and save it

**Cost estimate:** ~$0.50/day (24 articles × 1200 tokens × $0.002/1K tokens)

---

#### B) GitHub Token Credential

1. In n8n, go to **Credentials** → **New**
2. Search for "HTTP Header Auth"
3. Name it: `GitHub Token`
4. Configure:
   - **Name**: `Authorization`
   - **Value**: `token YOUR_GITHUB_TOKEN`
5. Save

**Create GitHub Personal Access Token:**
1. Go to https://github.com/settings/tokens
2. Click "Generate new token (classic)"
3. Name: `n8n-drmsi-workflow`
4. Scopes: Select `repo` (all sub-options)
5. Click "Generate token"
6. Copy the token (starts with `ghp_`)
7. Format in n8n: `token ghp_xxxxxxxxxxxxx`

---

#### C) Telegram Bot Credential (Optional)

1. In n8n, go to **Credentials** → **New**
2. Search for "Telegram"
3. Name it: `Telegram Bot`
4. Add your bot token

**Create Telegram Bot:**
1. Message @BotFather on Telegram
2. Send `/newbot`
3. Follow instructions
4. Copy the token
5. Get your chat ID: Message @userinfobot with `/start`

---

### Step 3: Import Workflow

1. Open n8n
2. Click **Workflows** → **Import from File**
3. Select `n8n-workflow-enhanced.json`
4. The workflow will load with 12 nodes

---

### Step 4: Update Credential References

After import, you need to link credentials:

**Node: "OpenAI Generate Article"**
1. Click the node
2. Under "Credentials", select `OpenAI account`
3. Save

**Node: "Push Article to GitHub"**
1. Click the node
2. Under "Credentials", select `GitHub Token`
3. Save

**Node: "Get Current index.html"**
1. Click the node
2. Under "Credentials", select `GitHub Token`
3. Save

**Node: "Push Updated index.html"**
1. Click the node
2. Under "Credentials", select `GitHub Token`
3. Save

**Node: "Notify Telegram"** (optional)
1. Click the node
2. Under "Credentials", select `Telegram Bot`
3. Save (or delete this node if not using)

---

### Step 5: Test Single Execution

Before activating the schedule:

1. Click **"Execute Workflow"** at the bottom
2. Wait 30-60 seconds
3. Check execution:
   - ✅ All nodes should be green
   - ✅ Check "Format Article HTML" node output
   - ✅ Verify GitHub shows new files

**Expected output:**
- 4 new files in `articles/` folder
- Updated `index.html` with 4 new article cards

---

### Step 6: Activate Schedule

1. Toggle the switch at top right: **"Inactive" → "Active"**
2. The workflow will now run every 4 hours automatically

**Schedule times (UTC):**
- 00:00, 04:00, 08:00, 12:00, 16:00, 20:00

**To change frequency:**
Edit the "Schedule Every 4 Hours" node and modify cron expression:
- Every 6 hours: `0 */6 * * *`
- Every 2 hours: `0 */2 * * *`
- Daily at noon: `0 12 * * *`

---

## 🎨 Article Output Examples

### Full Article Page (`articles/btc-analysis-2025-10-01.html`)

**Features:**
- Full HTML document with `<head>` and `<body>`
- SEO meta tags (description, keywords, OG tags)
- JSON-LD structured data
- Price badge (green/red with percentage)
- TradingView chart (4H interval, dark theme)
- Risk disclaimer at bottom
- Back to home link
- Mobile responsive

**URL format:** `https://drmsi.com/articles/btc-analysis-2025-10-01.html`

---

### Short Article Card (injected into `index.html`)

**Features:**
- Category icon (₿ for crypto, 💱 for forex, etc.)
- Date and category tags
- Title (H3)
- 150-char summary
- Price badge with change percentage
- "Read Analysis →" link (gold color)
- Hover effects

**Location:** Prepended to `<div class="articles-grid">` in index.html

**Latest 6 displayed:** Older articles automatically removed from homepage (files remain in `articles/` folder)

---

## 🔍 How It Works: Node-by-Node

### 1. Schedule Trigger
- Runs every 4 hours
- Cron: `0 */4 * * *`

### 2. Smart Symbol Selection
- Checks current UTC hour
- Selects 4 symbols based on time:
  - **00:00-06:00**: Crypto focus (BTC, ETH, SOL, XRP)
  - **06:00-12:00**: Forex + Gold (EUR/USD, GBP/USD, Gold, USD/JPY)
  - **12:00-18:00**: Stocks + Crypto (AAPL, MSFT, BTC, ETH)
  - **18:00-24:00**: Mixed (Gold, Silver, AMZN, SOL)
- Outputs 4 items with mapped Yahoo/TradingView symbols

### 3. Fetch Yahoo Finance Data
- Calls Yahoo Finance API for each symbol
- URL format: `https://query1.finance.yahoo.com/v8/finance/chart/SYMBOL?range=5d&interval=1d`
- Returns OHLCV data + metadata
- **Retry**: 3 attempts, 2-second delay

### 4. Process Market Data
- Extracts current price, change, percentage
- Calculates 5-day high/low
- Determines support/resistance levels
- Identifies trend (bullish/bearish)
- Formats prices based on asset type

### 5. OpenAI Generate Article
- **Model**: GPT-3.5 Turbo
- **Temperature**: 0.7 (balanced creativity)
- **Max tokens**: 1200 (≈400-500 words)
- **System prompt**: Professional analyst role
- **User prompt**: Includes all price data + requirements
- **Output**: Clean HTML (no DOCTYPE/html/body tags)

### 6. Format Article HTML
- Extracts title and meta description from AI content
- Creates slug: `symbol-analysis-date`
- Builds **full article HTML**:
  - Complete HTML document
  - SEO tags (meta, OG, Twitter, JSON-LD)
  - TradingView chart embed
  - Styled with dark theme CSS
- Builds **short card HTML**:
  - Article card for homepage
  - 150-char summary
  - Price badge
  - Read More link

### 7. Push Article to GitHub
- GitHub API: Create file
- Path: `articles/[slug].html`
- Content: Base64-encoded full article HTML
- Commit message: "Add article: [title]"

### 8. Get Current index.html
- GitHub API: Get file
- Path: `index.html`
- Returns content (base64) + SHA (for update)

### 9. Update index.html
- Decodes current index.html
- Finds `<div class="articles-grid">`
- Prepends new article card
- Keeps only 6 most recent cards (removes 7th+)
- Returns updated HTML + SHA

### 10. Push Updated index.html
- GitHub API: Update file
- Path: `index.html`
- Content: Base64-encoded updated HTML
- SHA: Required for update (from step 8)
- Commit message: "Update homepage with: [title]"

### 11. Notify Telegram (Optional)
- Sends notification to your Telegram
- Includes: Title, change %, article URL
- **Continue on fail**: Won't stop workflow if Telegram fails

---

## 🛠️ Customization Options

### Change Article Count per Run

Edit "Smart Symbol Selection" node, line 52-61:

```javascript
// Current: 4 symbols per run
selectedSymbols = ['BTCUSD', 'ETHUSD', 'SOLUSD', 'XRPUSD'];

// Change to 6 symbols:
selectedSymbols = ['BTCUSD', 'ETHUSD', 'SOLUSD', 'XRPUSD', 'XLMUSD', 'XAUUSD'];
```

### Change Articles Kept on Homepage

Edit "Update index.html" node, line 9:

```javascript
// Current: Keep 5 old + 1 new = 6 total
const cardsToKeep = existingCards.slice(0, 5);

// Change to 9 total:
const cardsToKeep = existingCards.slice(0, 8);
```

### Add More Symbols

Edit "Smart Symbol Selection" node, lines 3-47:

```javascript
'NZDUSD': { yahoo: 'NZDUSD=X', tradingview: 'FX:NZDUSD', name: 'NZD/USD', category: 'Forex' },
```

### Change AI Model

Edit "OpenAI Generate Article" node:
- **GPT-4**: More quality, higher cost ($0.03/1K tokens)
- **GPT-3.5**: Balanced, low cost ($0.002/1K tokens)

### Change TradingView Interval

Edit "Format Article HTML" node, line 180:

```javascript
"interval": "240",  // 4H (240 minutes)
// Options: "60" (1H), "D" (Daily), "W" (Weekly)
```

### Change Risk Disclaimer Text

Edit "Format Article HTML" node, around line 160 (in the `aiContent` section):

The AI will generate the disclaimer, but you can also add a fixed one in the template after the AI content.

---

## 🚨 Troubleshooting

### Issue: "OpenAI node failing"

**Solutions:**
1. Check API key is valid
2. Verify you have credits in OpenAI account
3. Check token limits (max 1200 should work)
4. Try GPT-3.5-turbo-0125 (newer version)

### Issue: "GitHub push failing"

**Solutions:**
1. Verify token format: `token ghp_xxxxx` (note the space)
2. Check token has `repo` scope
3. Verify `GITHUB_OWNER` and `GITHUB_REPO` env vars are correct
4. Check if `articles/` folder exists in repo (create manually first time)

### Issue: "Yahoo Finance returns no data"

**Solutions:**
1. Check symbol format (must match Yahoo Finance)
2. Try symbol directly in browser: `https://query1.finance.yahoo.com/v8/finance/chart/BTC-USD?range=5d`
3. Symbol might be delisted or invalid
4. Rate limit might be hit (workflow includes retry logic)

### Issue: "index.html injection not working"

**Solutions:**
1. Verify `articles-grid` div exists in index.html with exact ID
2. Check for closing comment: `</div><!-- articles-grid -->`
3. Manually verify structure in "Update index.html" node output
4. Make sure existing cards have proper `<article class="article-card">` tags

### Issue: "TradingView chart not loading"

**Solutions:**
1. Check TradingView symbol format (different from Yahoo)
2. Try symbol directly: https://www.tradingview.com/chart/?symbol=OANDA:XAUUSD
3. Some symbols require different exchanges (BINANCE, FX, NASDAQ, OANDA)
4. Check internet connection (charts load from TradingView CDN)

---

## 📊 Monitoring & Logs

### View Execution History
1. Go to **Executions** tab in n8n
2. Click any execution to see:
   - Node outputs
   - Execution time
   - Errors (if any)

### Check GitHub Commits
- Go to https://github.com/drmsi/drmsi.com/commits/main
- You should see commits every 4 hours:
  - "Add article: [title]"
  - "Update homepage with: [title]"

### Check Article Output
- Go to https://drmsi.com
- Homepage should show latest 6 articles
- Click "Read Analysis →" to see full article
- Verify TradingView chart loads

### Monitor Costs
- OpenAI: https://platform.openai.com/usage
- n8n Cloud: Check execution count (5K free/month)
- Expected: ~180 executions/month (6 per day × 30 days)

---

## 🎯 Next Steps

1. **Run test execution** and verify output
2. **Check first articles** on GitHub
3. **Activate schedule** for automated publishing
4. **Monitor for 24 hours** to ensure stability
5. **Adjust prompts** if article quality needs improvement
6. **Add more symbols** as needed
7. **Share articles** on social media (manual or add n8n nodes)

---

## 📞 Support

**Issues?**
- Check n8n execution logs first
- Verify all credentials are set correctly
- Test each node individually
- Check GitHub API responses

**Common fixes:**
- Re-import workflow if credentials not linking
- Restart n8n if changes not applying
- Clear browser cache if TradingView charts not loading

---

**Workflow created:** October 1, 2025
**Version:** Enhanced v2.0
**Compatibility:** n8n 1.0+ (tested with cloud and self-hosted)

**Ready to publish 24 SEO-optimized articles per day automatically!** 🚀
