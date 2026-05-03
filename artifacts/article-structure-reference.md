# DRMSI Article Structure Reference

## 📄 Two Article Formats

### 1. Full Article Page (`articles/[slug].html`)

**Complete standalone HTML file with:**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <!-- SEO Meta Tags -->
    <title>[Title] | DRMSI Trading Intelligence</title>
    <meta name="description" content="[150-160 char description]">
    <meta name="keywords" content="[symbol, category, trading terms]">
    <meta name="author" content="DRMSI Analysis Team">
    <link rel="canonical" href="https://drmsi.com/articles/[slug].html">

    <!-- Open Graph (Facebook/LinkedIn) -->
    <meta property="og:type" content="article">
    <meta property="og:title" content="[title]">
    <meta property="og:description" content="[description]">
    <meta property="og:url" content="https://drmsi.com/articles/[slug].html">
    <meta property="og:image" content="https://drmsi.com/og-image.jpg">

    <!-- Twitter Card -->
    <meta name="twitter:card" content="summary_large_image">
    <meta name="twitter:title" content="[title]">
    <meta name="twitter:description" content="[description]">

    <!-- JSON-LD Structured Data -->
    <script type="application/ld+json">
    {
      "@context": "https://schema.org",
      "@type": "Article",
      "headline": "[title]",
      "author": {"@type": "Organization", "name": "DRMSI Analysis Team"},
      "publisher": {"@type": "Organization", "name": "DRMSI"},
      "datePublished": "2025-10-01T12:00:00Z"
    }
    </script>

    <!-- Inline CSS (dark theme) -->
    <style>
        /* Full styling for article page */
    </style>
</head>
<body>
    <div class="container">
        <!-- Back link -->
        <a href="/" class="back-link">← Back to DRMSI</a>

        <article>
            <!-- Article header with metadata -->
            <header class="article-header">
                <div class="article-meta">
                    <span>📅 October 1, 2025</span>
                    <span>₿ Cryptocurrency</span>
                    <span class="price-badge">📈 +5.2%</span>
                </div>
            </header>

            <!-- AI-generated content -->
            <h1>[Article Title]</h1>
            <p>[Introduction...]</p>

            <h2>Market Overview</h2>
            <p>[Market analysis...]</p>

            <h2>Technical Analysis</h2>
            <p>[Technical details...]</p>

            <h2>Trading Outlook</h2>
            <p>[Outlook and predictions...]</p>

            <div class="risk-disclaimer">
                ⚠️ <strong>Risk Disclaimer:</strong> Trading involves risk...
            </div>

            <!-- TradingView Chart -->
            <div class="chart-container">
                <div id="tradingview_chart"></div>
                <script src="https://s3.tradingview.com/tv.js"></script>
                <script>
                new TradingView.widget({
                    "symbol": "BINANCE:BTCUSDT",
                    "interval": "240",
                    "theme": "dark"
                });
                </script>
            </div>
        </article>
    </div>
</body>
</html>
```

**File naming:** `[symbol]-analysis-[YYYY-MM-DD].html`
- Example: `btc-analysis-2025-10-01.html`
- Example: `eurusd-analysis-2025-10-01.html`

---

### 2. Short Article Card (injected into `index.html`)

**HTML snippet inserted into articles grid:**

```html
<article class="article-card" data-date="2025-10-01T12:00:00Z">
    <div class="article-image">
        <span style="font-size: 3rem;">₿</span>
    </div>
    <div class="article-content">
        <div class="article-meta">
            <span>📅 Oct 1</span>
            <span>🏷️ Cryptocurrency</span>
        </div>
        <h3>Bitcoin Price Analysis - Bullish Momentum Continues</h3>
        <p>Bitcoin shows strong bullish momentum with a 5.2% gain, breaking through key resistance at $64,500. Technical indicators suggest...</p>
        <div style="display: flex; justify-content: space-between; align-items: center; margin-top: 15px;">
            <span class="price-badge" style="background: #10b981; color: white; padding: 4px 10px; border-radius: 4px; font-size: 13px;">
                📈 +5.2%
            </span>
            <a href="/articles/btc-analysis-2025-10-01.html" style="color: #fbbf24; text-decoration: none; font-weight: 500;">
                Read Analysis →
            </a>
        </div>
    </div>
</article>
```

**Inserted location in index.html:**
```html
<div class="articles-grid" id="articles-grid">
    <!-- NEW ARTICLE PREPENDED HERE -->
    <article class="article-card">...</article>

    <!-- EXISTING ARTICLES (5 most recent kept) -->
    <article class="article-card">...</article>
    <article class="article-card">...</article>
    <!-- ... up to 6 total -->
</div><!-- articles-grid -->
```

---

## 🎨 Category Icons & Colors

### Icons
```javascript
const categoryIcons = {
  'Cryptocurrency': '₿',   // Bitcoin symbol
  'Forex': '💱',          // Currency exchange
  'Stock': '📊',          // Bar chart
  'Commodity': '🥇'       // Gold medal
};
```

### Price Badge Colors
```javascript
// Green for positive
background: #10b981;  // Bullish / Positive change

// Red for negative
background: #ef4444;  // Bearish / Negative change
```

---

## 📊 SEO Elements Explained

### 1. Meta Description (150-160 chars)
**Purpose:** Appears in Google search results
**Format:** `[Symbol] price analysis: Current price $X, [trend] trend with X% change. Technical analysis and trading outlook.`

**Example:**
```html
<meta name="description" content="Bitcoin price analysis: Current price $63,450, bullish trend with +5.2% change. Technical analysis, support/resistance levels, and trading outlook.">
```

### 2. Keywords
**Format:** `[Symbol name], [Symbol code], [Category], trading analysis, price analysis, technical analysis, market outlook`

**Example:**
```html
<meta name="keywords" content="Bitcoin, BTC, Cryptocurrency, trading analysis, price analysis, technical analysis, market outlook, support resistance">
```

### 3. Open Graph Tags
**Purpose:** Controls how links appear when shared on Facebook, LinkedIn, WhatsApp

**Key tags:**
- `og:title` - Article title
- `og:description` - Short description
- `og:image` - Preview image (you'll need to create og-image.jpg)
- `og:url` - Canonical URL
- `og:type` - Always "article"

### 4. Twitter Card
**Purpose:** Rich previews on Twitter/X

**Card type:** `summary_large_image`
- Shows large preview image
- Title, description, and image
- Increases engagement

### 5. JSON-LD Structured Data
**Purpose:** Helps Google understand your content for rich results

**Article schema includes:**
- headline
- author (Organization)
- publisher (with logo)
- datePublished
- dateModified
- mainEntityOfPage

**Benefits:**
- May appear in Google News
- Better search visibility
- Potential for rich snippets

### 6. Canonical URL
**Purpose:** Prevents duplicate content penalties

```html
<link rel="canonical" href="https://drmsi.com/articles/[slug].html">
```

---

## 📱 Responsive Design

### Mobile (<768px)
- Single column layout
- Larger touch targets
- Smaller font sizes
- Full-width chart

### Tablet (768-1024px)
- Two-column article grid on homepage
- Comfortable reading width
- Medium chart size

### Desktop (>1024px)
- Three-column article grid on homepage
- Max 900px article width for readability
- Large chart with controls

---

## 🔗 URL Structure

### Homepage
```
https://drmsi.com/
https://drmsi.com/index.html
```

### Articles
```
https://drmsi.com/articles/[symbol]-analysis-[date].html
```

**Examples:**
- `https://drmsi.com/articles/btc-analysis-2025-10-01.html`
- `https://drmsi.com/articles/eurusd-analysis-2025-10-01.html`
- `https://drmsi.com/articles/aapl-analysis-2025-10-01.html`

### Slug Format
```javascript
const slug = `${symbol.toLowerCase()}-analysis-${date}`;
// symbol: 'BTCUSD' → 'btcusd'
// date: '2025-10-01'
// Result: 'btcusd-analysis-2025-10-01'
```

---

## 📈 TradingView Chart Configuration

### Embedded Chart Settings
```javascript
new TradingView.widget({
    "width": "100%",
    "height": "100%",
    "symbol": "BINANCE:BTCUSDT",    // Changes per article
    "interval": "240",                // 4H (240 minutes)
    "timezone": "Etc/UTC",
    "theme": "dark",                  // Matches site theme
    "style": "1",                     // Candlestick
    "locale": "en",
    "toolbar_bg": "#1a1a1a",         // Dark background
    "enable_publishing": false,
    "hide_side_toolbar": false,
    "allow_symbol_change": true,      // Users can change symbol
    "container_id": "tradingview_chart"
});
```

### Interval Options
- `"60"` - 1 Hour
- `"240"` - 4 Hours (recommended)
- `"D"` - Daily
- `"W"` - Weekly

### Symbol Format (TradingView)
- **Forex:** `FX:EURUSD`
- **Crypto:** `BINANCE:BTCUSDT`
- **Stocks:** `NASDAQ:AAPL`
- **Commodities:** `OANDA:XAUUSD`

---

## ✅ Quality Checklist

Before publishing, verify:

### Content
- [ ] Title is SEO-optimized (60-70 chars)
- [ ] Meta description is 150-160 chars
- [ ] Introduction is engaging
- [ ] Has 3 main sections (Overview, Technical, Outlook)
- [ ] Includes risk disclaimer
- [ ] Professional tone, no hype

### Technical
- [ ] Valid HTML (no unclosed tags)
- [ ] All meta tags present
- [ ] JSON-LD validates (use Google's testing tool)
- [ ] TradingView chart loads
- [ ] Links work (back button, read more)
- [ ] Mobile responsive

### SEO
- [ ] Canonical URL set
- [ ] Open Graph tags complete
- [ ] Twitter Card configured
- [ ] Keywords naturally included
- [ ] Headers use proper hierarchy (H1 → H2 → H3)

### GitHub
- [ ] File pushed to `articles/` folder
- [ ] index.html updated with card
- [ ] Commit messages descriptive
- [ ] No merge conflicts

---

## 🎯 Performance Targets

### Page Load Time
- **Target:** <2 seconds (first contentful paint)
- **Optimizations:**
  - Inline CSS (no external stylesheet)
  - TradingView async load
  - Minimal JavaScript
  - Static HTML (no server processing)

### SEO Metrics
- **Title length:** 50-70 chars (optimal)
- **Meta description:** 150-160 chars (optimal)
- **First paragraph:** Under 100 words
- **Total word count:** 400-500 words
- **Readability:** Grade 10-12 (professional audience)

### Engagement Targets
- **Time on page:** 2-3 minutes
- **Bounce rate:** <60%
- **Social shares:** Enabled via OG tags
- **Mobile usability:** 100% (Google PageSpeed)

---

## 🚀 Going Live Checklist

1. **Create og-image.jpg**
   - Size: 1200×630px
   - Content: DRMSI logo + "Trading Intelligence"
   - Upload to repository root

2. **Test First Article**
   - Run workflow manually
   - Check article renders correctly
   - Verify chart loads
   - Test on mobile

3. **Validate SEO**
   - Google Rich Results Test: https://search.google.com/test/rich-results
   - Facebook Debugger: https://developers.facebook.com/tools/debug/
   - Twitter Card Validator: https://cards-dev.twitter.com/validator

4. **Submit to Search Engines**
   - Google Search Console: Submit sitemap
   - Bing Webmaster Tools: Submit URL
   - Request indexing for first few articles

5. **Monitor Performance**
   - Check n8n execution logs
   - Verify GitHub commits
   - Monitor website traffic
   - Track search rankings

---

**Last updated:** October 1, 2025
**Version:** 2.0 (Enhanced with full SEO)
