# Deploying this site to your VPS

This is a fully static site — no build step, no server-side code, no dependencies.
It's just HTML, CSS, and images.

## Deploying brokenmanstudios.com (this project's live site)

**Current host (since 2026-08-20):** a dedicated VPS (`217.154.231.122`)
running only this site — nginx, UFW (22/80/443 only), fail2ban,
unattended-upgrades, key-only SSH (root login and password auth both
disabled), Let's Encrypt via certbot with nginx's own auto-configured
redirect and auto-renewal (`certbot renew --dry-run` verified working).

**Previous/fallback host:** the Austria VPS also still has this site
deployed and configured (nginx + HAProxy SNI routing, its own Let's
Encrypt cert, auto-renewing). It is not attached to the domain right now
— DNS points at the dedicated VPS above — but it is a working manual
fallback. To fail over: log into the domain registrar (Porkbun) and point
`brokenmanstudios.com` / `www` A records back at the Austria VPS's IP,
then redeploy current `main` there if its copy has drifted (see "Local
deploy script" below — same script, different target).

A local-only `deploy.sh` (gitignored — see below) fetches `origin/main`,
exports it as a clean tarball (so Windows CRLF line endings from
`core.autocrlf` never leak into the deploy — see `.gitattributes`), rsyncs
it into the web root on the target VPS over an SSH key, and verifies
the live site matches `origin/main` byte-for-byte before finishing.

Only `index.html`, `privacy.html`, `studio-privacy.html`, `updates.html`, `updates/`, `robots.txt`, `sitemap.xml`,
`google851ba87864f359aa.html`, and `assets/` are deployed — `DEPLOY.md`,
`README.md`, and `SEO.md` stay in the repo and are never copied to the
web root.

**`deploy.sh` is intentionally not committed** (see `.gitignore`): it
contains SSH connection specifics the owner prefers to keep off a public
repo, even though the target IP is trivially discoverable via DNS anyway.
Keep the script local to the workstation(s) that need it; ask whoever set
it up for a copy rather than recreating it with real values committed to
git.

This is deliberately a manual, one-command step, not a GitHub Actions
auto-deploy on push — the owner decided against adding a standing deploy
credential to either VPS. Push to `main`, then run the local deploy
script when ready to go live.

## Files

```
index.html                        — homepage
privacy.html                      — QuiltMath privacy policy page
studio-privacy.html               — website and social presence privacy notice
updates.html                      — studio journal and project updates
updates/                          — individual Studio journal articles
robots.txt                        — crawl directives, points to sitemap.xml
sitemap.xml                       — lists all public HTML pages
google851ba87864f359aa.html       — Search Console ownership file, must stay published
assets/style.css                  — all styling (linked with a ?v= cache-busting query)
assets/screenshots/                — the 5 app screenshots, PNG + WebP (<picture> with WebP source, PNG fallback)
```

## Cache-busting after a CSS change

`assets/style.css` is served by nginx with `cache-control: max-age=604800` (7 days).
Editing the file in place will not reach visitors who already cached the old
version. After every CSS change, bump the query string on both HTML pages:

```html
<link rel="stylesheet" href="assets/style.css?v=3" />
```

(increment the number each time). `index.html`/`privacy.html` themselves are not
sent with a long `cache-control`, so the new link is picked up on next load.

## Quick deploy with Nginx (typical VPS)

1. Copy the whole `quiltmath-site` folder to your server, e.g.:
   ```
   scp -r quiltmath-site user@your-vps:/var/www/quiltmath
   ```

2. Point an Nginx server block at it:
   ```nginx
   server {
       listen 80;
       server_name yourdomain.com www.yourdomain.com;
       root /var/www/quiltmath;
       index index.html;

       location / {
           try_files $uri $uri.html $uri/ =404;
       }
   }
   ```

3. Reload Nginx: `sudo nginx -t && sudo systemctl reload nginx`

4. Add HTTPS with Certbot (free, Let's Encrypt):
   ```
   sudo certbot --nginx -d yourdomain.com -d www.yourdomain.com
   ```

## DNS

At your domain registrar, point an A record at your VPS's IP address
(and a CNAME for `www` if you want that too). Propagation is usually
minutes to a few hours.

## Before you go live — things to check/update

- Replace the "QM" text logo (`.brand-mark` in index.html / privacy.html)
  with a real icon/logo image if you have one.
- The three links in the "How to try QuiltMath" section point to the
  closed-testing Google Group and opt-in page. Once QuiltMath moves to
  a normal public release, simplify that whole section down to a single
  "Get it on Google Play" button/link.
- Double check the contact email and the effective date on the privacy
  page still match what's filed in Google Play Console.
