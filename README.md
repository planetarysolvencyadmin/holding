# Planetary Solvency holding page

A one-page [Jekyll](https://jekyllrb.com/) site: the brand image treatment (photo, frosted ring, logo), then "Launching in early November 2026" and the LinkedIn link. Plain CSS, one self-hosted font (Inter, OFL), no tracking, no cookies.

## Layout

| Path | Purpose |
| --- | --- |
| `index.html` | The page (content and hero markup) |
| `_layouts/default.html` | HTML shell, meta tags |
| `assets/css/style.css` | Styles; brand tokens copied from the main site |
| `assets/images/` | Two hero photos, `overlay.png` (ring, orbits, logo), `badge.png` (portrait version), favicon, social card |
| `_headers` | Cloudflare security and caching headers |

The hero picks one of the two photos at random on each load, as the main site does. To pin one, delete the `<script>` block at the bottom of `index.html` and edit the `<img id="hero-photo">` tag to the photo you want.

## Run locally

```sh
bundle install
bundle exec jekyll serve      # http://localhost:4000
```

## Publish on Cloudflare (Pages, connected to GitHub)

1. Merge your working branch into `main` (or choose the branch as the production branch in step 4).
2. In the [Cloudflare dashboard](https://dash.cloudflare.com/) go to **Workers & Pages** > **Create application** > **Pages** > **Connect to Git**. If the UI only offers Workers, look for the "Looking to deploy Pages?" link, or use the direct upload route below.
3. Authorise the Cloudflare GitHub app for the `holding` repository and select it.
4. Build settings:
   - **Framework preset:** Jekyll
   - **Build command:** `jekyll build`
   - **Build output directory:** `_site`
   - **Production branch:** `main`
5. Under **Environment variables** add `RUBY_VERSION` (for example `3.3.6`, matching `ruby -v` locally) for both Production and Preview. Cloudflare also reads a `.ruby-version` file.
6. **Save and Deploy.** The first build takes a minute or two and gives you a `*.pages.dev` address. Check it on a phone.
7. **Custom domain:** open the project > **Custom domains** > **Set up a custom domain**. If the domain's DNS is on Cloudflare it creates the records for you; otherwise follow the CNAME instructions shown. HTTPS certificates are automatic.
8. Optional: once live, set `url:` in `_config.yml` to the live address (no trailing slash). That switches on the social share image (`og:image`) so LinkedIn previews look right.

Every push to the production branch redeploys; other branches get preview URLs.

### Alternative: upload the built site without Git

```sh
bundle exec jekyll build
npx wrangler pages deploy _site --project-name planetary-solvency-holding
```

You will be prompted to log in to Cloudflare the first time.

### When the full site launches

Add the custom domain to the new site's project and remove it from this one. Nothing here needs deleting first.

## Sources and checks

- Cloudflare: [Deploy a Jekyll site](https://developers.cloudflare.com/pages/how-to/deploy-a-jekyll-site) and [build image](https://developers.cloudflare.com/pages/configuration/build-image/) (Ruby version handling).
- The photos and overlay were extracted from the agency's "Planetary Solvency image treatment" PDF. Confirm the licences cover web use (the main site's library credits Unsplash and Pexels).
