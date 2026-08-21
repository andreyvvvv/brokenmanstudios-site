# Broken Man Studios & QuiltMath Website

Official website and privacy policy for **Broken Man Studios** and **QuiltMath** Android application.

- **Live site:** [https://brokenmanstudios.com/](https://brokenmanstudios.com/)
- **Privacy Policy:** [https://brokenmanstudios.com/privacy.html](https://brokenmanstudios.com/privacy.html)
- **Studio updates:** [https://brokenmanstudios.com/updates.html](https://brokenmanstudios.com/updates.html)
- **Studio/site privacy:** [https://brokenmanstudios.com/studio-privacy.html](https://brokenmanstudios.com/studio-privacy.html)

## Structure

```text
├── index.html                        # Main landing page
├── privacy.html                      # Privacy Policy
├── studio-privacy.html               # Website and social presence privacy notice
├── updates.html                      # Studio journal and project updates
├── updates/                          # Individual Studio journal articles
├── robots.txt                        # Crawl directives
├── sitemap.xml                       # Sitemap for all public HTML pages
├── google851ba87864f359aa.html       # Google Search Console ownership file — do not delete
├── DEPLOY.md                         # Deployment notes
├── SEO.md                            # SEO work log and open items
├── assets/
    ├── style.css                     # Styling
    ├── logo.jpg / logo.png / logo.webp
    ├── screenshots/                  # App screenshots (PNG + WebP)
    └── updates/                      # Studio-journal artwork
```

## Deployment

Deployed to a dedicated VPS via Nginx on `brokenmanstudios.com` with Let's Encrypt SSL
(auto-renewing). Moved 2026-08-20 to an isolated VPS dedicated to this site only; the
previous host is kept configured as a manual fallback — see [DEPLOY.md](DEPLOY.md) for details.

## SEO

Search Console is verified and `sitemap.xml` is submitted. See [SEO.md](SEO.md) for
what's been done, current status, and what's still open.
