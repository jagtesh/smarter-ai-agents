# Site Archival & Content Cleanup

## Project Goal
Create a complete, offline-readable archive of protected web content with clean HTML, full-size images, and working internal navigation.

## Phase 1: Initial Setup & Authentication

**Objective:** Set up scraping infrastructure with proper authentication

**Key Requirements:**
- Use Playwright for browser automation (handles JavaScript-rendered content)
- Implement proper login flow:
  - Navigate to a protected page that triggers login
  - Identify correct form field selectors (inspect actual HTML, not assumptions)
  - Use `waitUntil: 'domcontentloaded'` instead of `'networkidle'` to avoid timeouts
  - Verify login success by checking for absence of restriction messages
- Maintain authenticated session across all page scrapes
- Add random delays (2-5 seconds) between requests to be respectful

**Common Issues to Avoid:**
- Wrong form selectors (check actual `name` attributes, not IDs)
- Timeout issues (use lighter wait conditions)
- Session loss (reuse browser context)

## Phase 2: Content Scraping

**Objective:** Download all pages and extract navigation links

**Key Requirements:**
- Start from main index/table of contents page
- Implement breadth-first crawling with URL queue
- Track visited URLs to avoid duplicates
- Extract internal links using specific selectors:
  - Main content links (e.g., `main > article > div > ul a`)
  - In-content links (e.g., `article a`)
- Filter links to only include relevant content (same domain/path pattern)
- Save complete HTML (for later processing)
- Implement retry logic (2-3 attempts) with exponential backoff
- Track failed URLs for later review

**Output Structure:**
```
output_dir/
├── page1.html
├── page2.html
├── images/
├── assets/
└── index.html (generated summary)
```

## Phase 3: Image Download & Optimization

**Objective:** Download full-resolution images, not thumbnails

**Critical Strategy:**
1. **Check parent anchor tags** - Often wrap `<img>` tags with links to full-size versions
2. **Parse srcset attributes** - Extract all size variants, download the largest
3. **Normalize filenames** - Strip dimension suffixes (e.g., `-680x381`) to create canonical filenames
4. **Download priority:**
   - First: Full-size from `<a href>` wrapper
   - Second: Largest from `srcset`
   - Third: Main `src` attribute

**Example Pattern:**
```html
<!-- Input -->
<a href="image.png">
  <img src="image-680x381.png"
       srcset="image-680x381.png 680w, image.png 1677w">
</a>

<!-- All download to: images/image.png (full 1677w version) -->
```

**Implementation:**
- Skip already-downloaded files
- Add timeout handling (30s per image)
- Silent failures for missing images (log but continue)

## Phase 4: HTML Cleanup & Path Fixing

**Objective:** Update all references to use local resources

**Transformations Needed:**

### 4.1 Image References
```javascript
// Pattern 1: Update <a href> to local full-size
href="https://site.com/.../image-680x381.png"
→ href="images/image.png"

// Pattern 2: Update <img src> to local full-size
src="https://site.com/.../image-680x381.png"
→ src="images/image.png"

// Pattern 3: Remove srcset (using full-size locally)
srcset="..." → (remove entirely)
```

### 4.2 Asset References
```javascript
// Update CSS links
href="https://site.com/.../style.css"
→ href="assets/style.css"
```

### 4.3 Internal Navigation Links
```javascript
// Update cross-references
href="https://site.com/page/article-name/"
→ href="article-name_article-title.html"
```

**Build URL Mapping:**
- Create Map of: `original_url → downloaded_filename`
- Apply bulk find/replace across all HTML files
- Handle both already-local and remote references

## Phase 5: HTML Simplification

**Objective:** Create minimal, fast-loading pages

**Extraction Strategy:**
1. **Keep minimal head:**
   ```html
   <head>
     <meta charset="UTF-8">
     <meta name="viewport" content="width=device-width, initial-scale=1">
     <title><!-- extracted from original --></title>
     <link rel="stylesheet" href="assets/SINGLE_STYLE.css">
   </head>
   ```

2. **Keep only body classes** (for CSS compatibility):
   ```html
   <body class="<!-- original classes -->">
   ```

3. **Extract main content only:**
   - Find main content container (e.g., `<main class="content">`)
   - Extract from opening tag to closing tag
   - Discard: navigation, sidebars, footers, scripts, widgets

4. **Minimal wrapper structure:**
   ```html
   <div class="site-container">
     <div class="site-inner">
       <div class="content-sidebar-wrap">
         <!-- main content here -->
       </div>
     </div>
   </div>
   ```

5. **Remove ALL JavaScript:**
   - Strip `<script>` tags
   - Strip inline `onclick` handlers
   - Strip tracking codes

6. **Update internal links to simplified versions:**
   ```javascript
   href="page.html" → href="simple-page.html"
   ```

## Phase 6: Cleanup & Optimization

**Final Steps:**
1. Delete redundant sized images (keep only full-size versions)
2. Generate master index.html with:
   - List of all pages
   - Scraping statistics
   - Failed URLs report
3. Remove unused CSS files (keep only 1 stylesheet)
4. Verify all internal links work

## Utility Scripts to Create

### 1. `main.js` - Full scraper
- Login → Crawl → Download images → Fix paths → Generate index

### 2. `fix-existing.js` - Repair existing downloads
- Scan HTML for missing images
- Download only what's missing
- Re-apply path fixes
- Don't re-scrape content

### 3. `fix-internal-links.js` - Update cross-references
- Build URL→filename mapping
- Update all `href` attributes
- Handle edge cases (external links, anchors)

### 4. `simplify-html.js` - Content extraction
- Extract main content only
- Apply minimal template
- Update to simple-* filenames
- Strip all cruft
- Modify links to point to other simple pages

## Error Handling Patterns

```javascript
// Pattern 1: Graceful degradation
try {
  const result = await riskyOperation();
} catch (error) {
  console.warn(`Failed but continuing: ${error.message}`);
  // Don't throw - continue processing
}

// Pattern 2: Retry with backoff
async function withRetry(fn, maxRetries = 3) {
  for (let i = 0; i < maxRetries; i++) {
    try {
      return await fn();
    } catch (error) {
      if (i === maxRetries - 1) throw error;
      await delay(Math.pow(2, i) * 1000);
    }
  }
}

// Pattern 3: Track failures for later
if (failed) {
  this.failedUrls.push(url);
  // Process at end or manually
}
```

## Testing Strategy

**Always test on small sample first:**
1. Create test script with `maxPages: 3` limit
2. Verify login works (check screenshot)
3. Verify images download at full resolution
4. Verify paths are correctly updated
5. Verify simplified HTML looks correct
6. **Only then** run full scrape

## Key Files Structure

```
project/
├── main.js                 # Full scraper
├── test.js                # Limited test run (3 pages)
├── fix-existing.js        # Repair utility
├── fix-internal-links.js  # Link updater
├── simplify-html.js       # Content extractor
├── package.json           # Dependencies
└── output_dir/
    ├── *.html            # Original downloads
    ├── simple-*.html     # Cleaned versions
    ├── images/           # Full-size only
    ├── assets/           # CSS
    └── index.html        # Generated index
```

## Quick Start Checklist

- [ ] Install Playwright: `npm install playwright`
- [ ] Identify login selectors (inspect form fields)
- [ ] Find main content container selector
- [ ] Test login flow on 1 page
- [ ] Test image download (verify full-size)
- [ ] Test on 3 pages
- [ ] Run full scrape
- [ ] Fix any broken images
- [ ] Simplify HTML
- [ ] Verify all links work

---

**Use this template whenever you need to:**
- Archive protected/members-only content
- Download full-resolution images (not thumbnails)
- Create offline-readable documentation
- Clean up bloated HTML to essentials
- Fix broken local references

**Adapt by:**
- Adjusting selectors for target site structure
- Modifying login flow for different auth methods
- Customizing filename patterns
- Adjusting content extraction rules
