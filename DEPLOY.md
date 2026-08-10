# Deploying Moving Billboards to Vercel

The website lives in the **`Moving Billboards/`** folder of this repo. It is a
static site (HTML/CSS/JS, no build step), so deployment is fast and free on
Vercel's Hobby plan.

## Current status: the site is offline

Every URL — including the homepage — now serves `Moving Billboards/404.html`
with a real HTTP 404. Nothing was deleted: `index.html` and the rest of the
site are still in this repo, they are just listed in
`Moving Billboards/.vercelignore` so Vercel never uploads them. `robots.txt`
also disallows all crawling, and `sitemap.xml` is no longer served.

**To bring the site back:** delete `Moving Billboards/.vercelignore`, restore
`robots.txt` to `Allow: /` with the sitemap line, and redeploy. `404.html` can
stay — with `index.html` deployed again it goes back to being an ordinary
not-found page for unknown URLs.

## 1. Import the project

1. Go to <https://vercel.com/new> and import this GitHub repo.
2. On the configure screen, set:
   - **Framework Preset:** `Other`
   - **Root Directory:** `Moving Billboards`  ← click *Edit* and pick the folder
   - **Build Command:** leave empty
   - **Output Directory:** leave empty
3. Click **Deploy**. Vercel serves `index.html` from that folder.

> The `Root Directory = Moving Billboards` step is important — the site files
> are inside that subfolder, not at the repo root. `vercel.json`, `robots.txt`
> and `sitemap.xml` all live in there too.

## 2. Add your domain

You already own **moving-billboards.co.za**.

1. In the Vercel project → **Settings → Domains**, add:
   - `moving-billboards.co.za` (set as the primary domain)
   - `www.moving-billboards.co.za` (Vercel will offer to redirect it to the apex — accept)
2. Vercel shows the DNS records to set at your `.co.za` registrar:
   - Apex `moving-billboards.co.za` → **A** record `76.76.21.21`
     (or an **ALIAS/ANAME** to `cname.vercel-dns.com` if your registrar supports it)
   - `www` → **CNAME** `cname.vercel-dns.com`
3. Wait for DNS to propagate; Vercel issues an SSL certificate automatically.

`vercel.json` already redirects `www` → the apex domain, so you have one
canonical URL (good for SEO).

## 3. SEO — so the site shows up when people search "Moving Billboards"

The static HTML now includes everything search engines need:

- A real `<title>` and meta description
- Open Graph + Twitter cards (nice link previews on WhatsApp / Facebook / X)
- `LocalBusiness` structured data (JSON-LD) with your name, phone, email and area served
- A `<noscript>` block with the core content, so crawlers see real text even
  before the JavaScript renders
- `robots.txt` and `sitemap.xml`
- A canonical URL (`https://moving-billboards.co.za/`)

### After the domain is live, do these once:

1. **Google Search Console** — <https://search.google.com/search-console>
   - Add the property `moving-billboards.co.za` and verify (DNS TXT is easiest).
   - Submit the sitemap: `https://moving-billboards.co.za/sitemap.xml`
   - Use **URL Inspection → Request indexing** on the homepage to get crawled fast.
2. **Google Business Profile** — <https://www.google.com/business/>
   - Create a free listing for "Moving Billboards" (service-area business,
     Sandton/Randburg). This is the single biggest lever for a local business
     name search and puts you on Google Maps.
3. **Bing Webmaster Tools** — <https://www.bing.com/webmasters> — submit the same sitemap.
4. Add the site link anywhere you can (WhatsApp business profile, any social
   pages, directories). Inbound links + the exact business name in your title
   are what make you rank #1 for your own brand name.

> Ranking for your own brand name ("Moving Billboards") is normally quick once
> Google has crawled and indexed the site and you have a Business Profile —
> usually days to a couple of weeks. The generic phrase "moving billboards"
> (vehicle advertising in general) is more competitive and builds over time.

## Local preview

```bash
cd "Moving Billboards"
python3 -m http.server 8000
# open http://localhost:8000
```
