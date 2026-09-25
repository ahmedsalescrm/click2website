# Click2Website — homepage

Single-file site: `index.html` plus an `assets/img` folder for screenshots. No build step, no framework, no server code.

## Deploy on GitHub Pages

1. Create a new repository (for example `click2website`) and upload everything in this folder, keeping the structure:
   ```
   index.html
   assets/img/…      ← your screenshots (see assets/img/README.md)
   logo/…            ← brand files, not used by the page itself
   .nojekyll
   ```
2. In the repository go to **Settings → Pages**, set **Source** to *Deploy from a branch*, pick `main` and `/ (root)`, and save.
3. After a minute the site is live at `https://<your-username>.github.io/<repo>/`.
4. Custom domain (`click2website.site`): in **Settings → Pages → Custom domain** enter `www.click2website.site`. GitHub then creates a `CNAME` file for you. At your DNS provider, point `www` to `<your-username>.github.io` with a CNAME record, and the apex (`@`) to GitHub's A records (185.199.108.153, 185.199.109.153, 185.199.110.153, 185.199.111.153). Tick **Enforce HTTPS** once the certificate is issued.

The site also works on any other static host (Netlify, Cloudflare Pages, cPanel): upload the same files.

## Before going live

- **Add your real screenshots** to `assets/img` (filenames in `assets/img/README.md`). Until then the page shows built-in stand-in designs for each client site.
- **Remove the stand-ins once your screenshots are in.** Open `index.html`, find the comment `PREVIEW MOCKUPS` near the bottom, and delete that single `<script>` line. The file drops from about 740 KB to about 180 KB.
- Enable gzip or Brotli on your host if it isn't on by default (GitHub Pages does this automatically).

## Editing content

Everything is plain HTML in `index.html`, top to bottom:

- **Hero** — the `<section class="hero-pin">` block: headline, sub copy, buttons, the stats line, and the closing line shown at the end of the fly-through.
- **3D fly-through** — the list of sites is `PROJECTS` in the script at the bottom (`slug` + image path). Add or remove entries there; the same slugs are used by the stand-in block.
- **Live sites marquee** — the two `<ul class="mq">` lists (keep both identical; the second one is the seamless repeat).
- **Services / Work / How we work / Client stories** — normal HTML sections, each marked with a `============` comment.
- **Contact** — the form opens WhatsApp (or email) with the details filled in. Change the number in the two `wa.me/923194884054` links and the `emailInstead` handler if it changes; the email address is `Ashfan354@gmail.com` in the same places.
- **Colours and fonts** — the `:root` tokens at the top of the `<style>` block. Gold is `--gold`; the light and dark palettes are both there. The font is Geist from Google Fonts.

## Brand files

`logo/` contains the new mark and lockup as SVG (text outlined, no font needed) and PNG, in dark and white versions. The favicon is embedded in `index.html`.

## Performance notes

- The 3D scene is raw WebGL (no library): one shader, one draw call per card, textures shared between cards, resolution capped at 1.25× on phones and 1.5× on desktop.
- It renders only while the hero is on screen and the tab is visible, drops to 30fps when nothing is moving, and lowers its own resolution if a device can't keep up.
- All other motion is transform/opacity only; images below the fold are lazy-loaded; GSAP is inlined so there are no third-party script requests.
- Visitors with "reduce motion" enabled get a still scene; browsers without WebGL get the page without it.
