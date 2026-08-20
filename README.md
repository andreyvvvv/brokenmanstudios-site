# Broken Man Studios & QuiltMath Website

Official website and privacy policy for **Broken Man Studios** and **QuiltMath** Android application.

- **Live site:** [https://brokenmanstudios.com/](https://brokenmanstudios.com/)
- **Privacy Policy:** [https://brokenmanstudios.com/privacy.html](https://brokenmanstudios.com/privacy.html)

## Structure

```text
├── index.html                        # Main landing page
├── privacy.html                      # Privacy Policy
├── robots.txt                        # Crawl directives
├── sitemap.xml                       # Sitemap (index.html + privacy.html)
├── google851ba87864f359aa.html       # Google Search Console ownership file — do not delete
├── DEPLOY.md                         # Deployment notes
├── SEO.md                            # SEO work log and open items
└── assets/
    ├── style.css                     # Styling
    ├── logo.jpg / logo.png / logo.webp
    └── screenshots/                  # App screenshots (PNG + WebP)
```

## Deployment

Deployed to a dedicated VPS via Nginx on `brokenmanstudios.com` with Let's Encrypt SSL
(auto-renewing). Moved 2026-08-20 to an isolated VPS dedicated to this site only; the
previous host is kept configured as a manual fallback — see [DEPLOY.md](DEPLOY.md) for details.

## SEO

Search Console is verified and `sitemap.xml` is submitted. See [SEO.md](SEO.md) for
what's been done, current status, and what's still open.
