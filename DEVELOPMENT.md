# Reference Validator - Development Log

## Project Overview

**Project Name:** Reference Validator (文献真伪验证器)
**Domain:** https://ai2paper.com
**Repository:** https://github.com/JohnnyChen1113/ref-validator
**Hosting:** Vercel
**First Created:** December 18, 2025

### Purpose
A single-page web application to verify AI-generated academic references against public databases. Helps detect "hallucinated" citations that AI models sometimes generate.

---

## Tech Stack

| Component | Technology |
|-----------|------------|
| Frontend | Single HTML file, vanilla JavaScript |
| Styling | Tailwind CSS (CDN) |
| APIs | Crossref, OpenAlex, Semantic Scholar, PubMed, Europe PMC |
| Hosting | Vercel |
| Analytics | Google Analytics 4 + Vercel Analytics |
| Domain | ai2paper.com (via Vercel DNS) |

---

## Core Features

### 1. Multi-Database Verification
- **Round 1 (Fast):** Crossref + OpenAlex
- **Round 2 (Comprehensive):** Semantic Scholar, PubMed, Europe PMC
- Two-round strategy for efficiency

### 2. Citation Format Support
- APA, MLA, Chicago, Vancouver, BibTeX
- DOI extraction (10.xxxx/xxxx)
- arXiv ID extraction (arXiv:xxxx.xxxxx)
- PMID extraction (PMID: xxxxxxxx)

### 3. Title Similarity Matching
- Jaccard-like algorithm
- 0.5 threshold for partial match
- Handles case differences and minor variations

### 4. User Interface
- Dark/Light theme toggle
- Chinese/English i18n (default: English)
- Line numbers in textarea (no word wrap)
- Card view and Table view for results
- "Select All" databases button
- Quick copy buttons (Verified, Partial, Not Found, All)

---

## File Structure

```
ref-validator/
├── index.html          # Main application (single-page)
├── og-image.png        # Social sharing image (1200x630)
├── og-image.svg        # Source SVG for og-image
├── sitemap.xml         # SEO sitemap
├── robots.txt          # Crawler instructions
├── .gitignore          # Git ignore rules
├── DEVELOPMENT.md      # This file
├── test_data.txt       # Test dataset (10 real + 10 fake refs)
├── test_simple.txt     # Simplified test file
└── ava_ref.md          # Initial reference notes
```

---

## Development History

### Session 1: Initial Build
- Created single-page HTML application
- Integrated 5 academic databases via public APIs
- Implemented citation parsing for multiple formats
- Added two-round verification strategy
- Built card-based results display

### Session 2: UI Improvements
- Added table view with verification evidence
- Shows "Input Title" vs "Found Title" comparison
- Color coding: green (exact), yellow (partial), gray (not found)
- Added match score percentage display

### Session 3: UX Enhancements
- **Line Numbers:** Added to textarea for clarity
- **No Word Wrap:** Each reference stays on single line
- **Select All Button:** One-click to enable all databases
- **Default Language:** Changed from Chinese to English

### Session 4: SEO & Deployment
- Added comprehensive SEO meta tags
- Open Graph tags for Facebook/LinkedIn
- Twitter Card meta tags
- JSON-LD structured data (WebApplication schema)
- Created og-image.png for social sharing
- Added sitemap.xml and robots.txt
- Deployed to Vercel
- Connected ai2paper.com domain (Vercel DNS)

### Session 5: Analytics
- Added Google Analytics 4 (G-5333T67WN9)
- Added Vercel Analytics

---

## API Endpoints Used

| Database | API | Rate Limit |
|----------|-----|------------|
| Crossref | `api.crossref.org/works` | Polite pool (email in header) |
| OpenAlex | `api.openalex.org/works` | 100k/day free |
| Semantic Scholar | `api.semanticscholar.org/graph/v1` | 100/5min |
| PubMed | `eutils.ncbi.nlm.nih.gov/entrez` | 3/sec without key |
| Europe PMC | `www.ebi.ac.uk/europepmc/webservices` | Generous |

---

## Key Design Decisions

### 1. Two-Round Strategy
**Why:** Crossref + OpenAlex cover most academic papers. Only query slower/rate-limited APIs (PubMed, Semantic Scholar) if needed.

### 2. Title-Based Matching
**Why:** DOI extraction fails for many citation formats. Title similarity matching is more robust as fallback.

### 3. Single HTML File
**Why:** No build process, easy to deploy, no dependencies. CDN for Tailwind CSS.

### 4. Evidence Display in Table
**Why:** Users need to see WHY a reference was marked as partial/not found. Comparing input vs found title makes verification transparent.

---

## Known Issues & Limitations

1. **False Positives:** Title similarity can match wrong papers with similar titles (~57% score)
2. **Rate Limiting:** Semantic Scholar has strict limits (100 requests/5 minutes)
3. **CORS:** All APIs used support CORS or have public endpoints
4. **Emoji Favicon:** May not display on all browsers

---

## Future Improvements

- [ ] Export results to CSV/BibTeX
- [ ] Batch processing for large lists
- [ ] API key support for higher rate limits
- [ ] More languages (Japanese, Spanish)
- [ ] Browser extension version
- [ ] Adjustable similarity threshold
- [ ] Citation format auto-detection confidence

---

## DNS Configuration (ai2paper.com)

**Nameservers:** (set in Spaceship)
- ns1.vercel-dns.com
- ns2.vercel-dns.com

**DNS Records:** (managed by Vercel)
- ALIAS → cname.vercel-dns.com (auto-managed)
- CAA → letsencrypt.org (SSL)

---

## Analytics Setup

### Google Analytics 4
- Measurement ID: `G-5333T67WN9`
- Dashboard: https://analytics.google.com

### Vercel Analytics
- Enable in Vercel Dashboard → Analytics → Enable
- Provides Web Vitals (LCP, FID, CLS)

---

## Useful Commands

```bash
# Start local server for testing
python3 -m http.server 8765

# Check DNS propagation
nslookup ai2paper.com

# Deploy (automatic via GitHub push)
git add -A && git commit -m "message" && git push
```

---

## Contact & Links

- **GitHub Issues:** https://github.com/JohnnyChen1113/ref-validator/issues
- **Live Site:** https://ai2paper.com
- **Vercel Dashboard:** https://vercel.com/dashboard

---

*Last Updated: December 18, 2025*
