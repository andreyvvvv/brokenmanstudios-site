# Deploying this site to your VPS

This is a fully static site — no build step, no server-side code, no dependencies.
It's just HTML, CSS, and images.

## Files

```
index.html                        — homepage
privacy.html                      — privacy policy page
robots.txt                        — crawl directives, points to sitemap.xml
sitemap.xml                       — lists index.html and privacy.html
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
