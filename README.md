# BDOFFER

Static Bengali mobile-offer landing page for Banglalink, Grameenphone, Robi, and Airtel.

## GitHub Pages

This repository includes a GitHub Actions workflow at `.github/workflows/pages.yml` that deploys the site to GitHub Pages whenever changes are pushed to `main`.

After the first push, enable **Settings → Pages → Source: GitHub Actions** if GitHub has not enabled the Pages site automatically. The project-site URL will be:

`https://atik336666-sketch.github.io/bdoffer/`

The site is static-only and does not require a build step or server runtime.

## Cache busting for link updates

The public-link request uses both `cache: 'no-store'` and a timestamp query parameter (`?ts=...`) so a CDN or intermediary cannot reuse an older API response. The landing-page bundle is also loaded with a versioned URL in `index.html`; increment the `v` value whenever the JavaScript bundle changes.

The API server is not part of this static repository. Add the following middleware in the API server before the `/api/public-links` and `/api/admin/*` route declarations (for an Express server):

```js
app.use('/api', (req, res, next) => {
  res.set({
    'Cache-Control': 'no-store, no-cache, must-revalidate, proxy-revalidate',
    Pragma: 'no-cache',
    Expires: '0'
  });
  next();
});
```

If the API uses a route-specific handler instead, set the same headers inside the `GET /api/public-links` handler before sending JSON. The API currently runs outside this repository, so this server-side change must be made in that API project's source or hosting configuration.
