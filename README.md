# Three Hands HTTPS mask

This public GitHub Pages wrapper keeps `threehands.dev` in the browser address
bar while displaying the site hosted by the isolated ThreeHands Tailscale
Funnel at `threehands-website.tail220731.ts.net`.

The website itself and its continuous deployment remain in the private
`ThreeHandsCo/Website` repository and the Docker deployment host.

## Important distinction

This is a full-screen, cross-origin iframe—not a reverse proxy. GitHub Pages
terminates HTTPS for `threehands.dev`; the application itself is loaded from
the Funnel origin.

## Required GitHub Pages settings

- Source: `main`, repository root (`/`)
- Custom domain: `threehands.dev`
- Enforce HTTPS: enabled
- `CNAME` contents: `threehands.dev`

The iframe must retain:

```html
allow="autoplay; fullscreen; picture-in-picture"
```

The application relies on that policy for muted media playback inside the
cross-origin frame.

## If the Funnel hostname changes

1. Verify the replacement Funnel origin directly.
2. Update the iframe, favicon, and noscript URLs in `index.html`.
3. Push `main` and wait for the GitHub Pages build.
4. Verify `https://threehands.dev` in real mobile and desktop browsers.

## Verification

The mask and origin are separate failure domains. Test both:

```sh
curl -fsS https://threehands-website.tail220731.ts.net/healthz
curl -fsSI https://threehands.dev/
```

An HTTP success from GitHub Pages does not prove that the nested Funnel frame
loaded, so finish with a browser test.

The complete private bootstrap, rollback, credential-rotation, and recovery
runbook is `.deployment/README.md` in `ThreeHandsCo/Website`.
