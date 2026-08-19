# Deploying this site to your VPS

This is a fully static site — no build step, no server-side code, no dependencies.
It's just HTML, CSS, and images.

## Files

```
index.html            — homepage
privacy.html           — privacy policy page
assets/style.css       — all styling
assets/screenshots/    — the 5 app screenshots
```

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
