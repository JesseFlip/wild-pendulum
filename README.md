# Zero Velocity

An unofficial, independent interactive study guide to ideas in Itzhak Bentov's
*Stalking the Wild Pendulum*. Not affiliated with the author or publisher.

Live at <https://wild-pencil.netlify.app>.

## Layout

The deployable site is a **pre-built** Vite bundle committed under
`zero-velocity-site/`. There is no source tree and no build step in this
repository — Netlify publishes that folder as-is.

```
netlify.toml           deploy config (publish dir, SPA rewrite, headers)
zero-velocity-site/    <- the publish directory
  index.html
  assets/              content-hashed JS/CSS/fonts
  _redirects           SPA fallback (used by drag-and-drop deploys)
  _headers             security + caching headers (mirrors netlify.toml)
  robots.txt sitemap.xml favicon.svg og.png licenses/
```

## Deploying

Netlify is Git-connected to this repository. `netlify.toml` sets
`publish = "zero-velocity-site"`; **without it Netlify serves the repository
root, which has no `index.html`, and every URL returns a 404.**

Netlify site settings must be:

| Setting            | Value                 |
| ------------------ | --------------------- |
| Base directory     | *(empty)*             |
| Build command      | *(empty)*             |
| Publish directory  | `zero-velocity-site`  |
| Production branch  | `main`                |

`netlify.toml` supplies the build/publish values, so leaving the UI fields
empty is fine — but a **non-empty base directory in the UI overrides this file**
and will break the deploy again.

The app is client-side routed, so the `/* -> /index.html 200` rewrite is
required; without it `/waves`, `/pendulum` etc. 404 on direct load or refresh.

## Verifying locally

```sh
cd zero-velocity-site
python3 -m http.server 8899   # then open http://127.0.0.1:8899
```

Plain `http.server` does not do the SPA rewrite, so deep links such as
`/waves` will 404 locally even though they work once deployed. Use
`npx serve -s .` to emulate the fallback.

## Absolute URLs

`sitemap.xml`, `robots.txt` and the `og:`/`twitter:` meta tags in `index.html`
hard-code `https://wild-pencil.netlify.app`. Update all three if the site moves
to a custom domain.
