# Nicole Slohoda Team — nicoleslohodateam.com deployment

Generated 2026-05-17 from Nicole's 2026-05-14 change list + intake worksheet.

## What's in this build

| Change Nicole asked for | Status in `index.html` |
|---|---|
| Use the standard headshot for ALL photos | ✅ Single image referenced twice (hero + about) — already pointed at the live `wp-content/uploads/2026/05/ChatGPT-Image-Mar-24-2026-06_55_03-PM.png` she has on the site |
| Add KW Elite Realtors office logo | ✅ Trust bar + footer compliance line — `/wp-content/uploads/kw-elite-logo.png` |
| Add Nicole Slohoda Team logo | ✅ Slot ready as `/wp-content/uploads/team-logo.png` (currently used as text mark in nav/footer — swap to the file once uploaded) |
| Color scheme: **Red, Black, Grey, White** | ✅ `--red:#b40101`, `--black:#000`, `--grey-dark:#4d4d4d`, `--grey:#828282`, `--white:#fff` — exactly the hex codes from her intake |
| Remove the Membership section | ✅ Section, nav link, and dedicated page all removed |
| Connect listings / IDX | ✅ Featured Listings section + 8 community deep-links + live `<iframe>` to `nicoleslohoda.kw.com` |
| Cell + Office + Email + Cal.com | ✅ Contact section + footer wired to her real numbers |
| Service area copy | ✅ Central NJ · Middlesex · Union · Woodbridge Township |
| All social links from intake | ✅ Instagram, Facebook, LinkedIn, YouTube, Zillow, Realtor.com |
| NJ compliance | ✅ License #2078986, brokerage address, NJ Consumer Information Statement link, EHO + REALTOR® badges |
| Mortgage calculator | ✅ Working JS, no third-party deps |
| Testimonials carousel | ✅ Auto-rotating, 3 placeholder slots — **swap in real Zillow / Google reviews** |
| Membership page | ❌ Removed (per her note: "till i get that all situated and ready to go") |

## Step 1 — No logo uploads required

The homepage and Virtual Staging page no longer depend on any externally uploaded logo files. The KW Elite brokerage reference in the trust bar uses an inline SVG building icon + text label, matching the format of the other trust items. The headshot is already live on her WP. Skip to Step 2.

## Step 2 — Replace the homepage

You have two clean paths. Pick whichever matches her current theme.

### Path A (cleanest) — full-page custom template

1. Hostinger File Manager (or SFTP) → `/wp-content/themes/<active-theme>/`
2. Upload `index.html` (this file's sibling) as `page-home.php` — wrap the body content with:
   ```php
   <?php /* Template Name: Nicole Home */ ?>
   <!DOCTYPE html>
   ... <!-- paste the rest of index.html verbatim --> ...
   ```
3. WP admin → **Pages → Add New** → Title "Home" → on the right, **Template = Nicole Home** → Publish.
4. **Settings → Reading → Your homepage displays = A static page → Homepage = Home**.

### Path B (fastest) — Custom HTML block on existing Home page

1. WP admin → **Pages → All Pages → edit "Home"** (or whichever page is set as the front page).
2. Switch the editor to **Code editor** (top-right ⋮ menu).
3. Replace the entire page contents with the **`<body>` section only** from `index.html` (lines starting at `<nav id="topnav">` and ending at `</footer>`).
4. Move the `<style>` block and the closing `<script>` block into the theme's `Appearance → Customize → Additional CSS` (CSS only) and a **Code Snippets** plugin for the JS (or paste the script tag inside a Custom HTML block at the bottom of the page).
5. **Settings → Reading → Your homepage displays = A static page → Homepage = Home**.

> Path A is preferred — single file, no CSS bleed from the theme. Path B works if the existing theme can't be touched.

## Step 3 — Redirect `nicoleslohoda.kw.com` → `thenicoleslohodateam.com`

Nicole asked for this so her business cards keep working. The subdomain `nicoleslohoda.kw.com` is hosted by **Keller Williams Command (kvCORE)** — you cannot redirect it from WordPress.

Options, in order of effort:

1. **KW Command (preferred)** — log into Command → **Marketing → Websites → Settings → Forwarding URL** → enter `https://nicoleslohodateam.com/`. If your tier doesn't expose forwarding, skip to #2.
2. **kvCORE Sphere / kvCORE Support ticket** — kw.com Command support (`support@kw.com`) can flip a 301 on the agent subdomain. Open a ticket: "Please 301-redirect `nicoleslohoda.kw.com` to `https://nicoleslohodateam.com/`. Reference KW Command #21297638."
3. **Soft redirect via JS** — set the kvCORE site's homepage **About** description / banner CTA to point to `nicoleslohodateam.com` so traffic clicks through. Not a true 301 but loses very little.

Also redirect (Hostinger → Domains → DNS / Redirects):
- `www.nicoleslohodateam.com` → `nicoleslohodateam.com` (or vice versa — pick one)
- Old vanity domains from intake: handle Substack on substack.com side (`@nicoleslohodarealtor → nicoleslohodateam.com` link in profile)

## Step 4 — Wire forms to her CRM

The contact form currently falls back to `mailto:` (opens her mail client). Production options:

- **kvCORE webhook** (best — leads land directly in Command):
  - Command → Marketing → Lead Engine → External Forms → copy webhook URL.
  - Replace `submitContact()` body with:
    ```js
    fetch('https://api.kvcore.com/v2/external/forms/<YOUR-FORM-ID>', {
      method: 'POST', headers: {'Content-Type':'application/json'}, body: JSON.stringify(Object.fromEntries(data))
    }).then(r => alert('Thanks! Nicole will be in touch shortly.'));
    ```
- **WPForms / Fluent Forms** plugin — drop the plugin in, rebuild the form once in their UI, replace the `<form>` block with the shortcode.
- **Formspree / Basin** — quickest, $0–$10/mo. Set `action="https://formspree.io/f/<id>"` and `method="POST"`.

## Step 5 — Verify before going live

Open `https://nicoleslohodateam.com/?preview=true` and check:

- [ ] Headshot renders in hero + about
- [ ] KW Elite logo renders in trust bar + footer
- [ ] All 6 social links open in new tabs (Instagram, Facebook, LinkedIn, YouTube, Zillow, Realtor.com)
- [ ] Phone numbers tap-to-call on iPhone
- [ ] `mailto:` opens for the email link
- [ ] Mortgage calculator updates as you type
- [ ] Testimonial carousel auto-rotates every 7s
- [ ] `nicoleslohoda.kw.com` iframe loads (or, if KW blocks framing, the deep-link cards still work)
- [ ] License #2078986 visible in trust bar + about + footer
- [ ] NJ Consumer Information Statement link works in the footer

## Step 6 — Submit Bridge Interactive / All Jersey MLS IDX (already in progress)

Once approved (Nicole expects MMSI / Bridge to contact her — see the "Quick Heads Up" email I sent her on 2026-04-10), swap the iframe + deep-link cards in the Featured Listings section for a true RESO Web API widget. IDX Broker, Showcase IDX, and direct kvCORE Sphere embeds all work. We'll do this when the approval lands.

## Step 7 — Deploy the Virtual Staging page

A second standalone page (`virtual-staging.html`) is in this folder. Same brand language as the homepage, same nav and footer — but no shared CSS file dependency, it's a true single-file drop-in.

### Path A — Static drop-in folder (fastest, works with WP)

WordPress's mod_rewrite only kicks in for non-existent files, so a static file in a subfolder is served by Apache before WP sees the request.

1. Hostinger File Manager → `public_html/`
2. Create folder `virtual-staging`
3. Upload `virtual-staging.html` into that folder → **rename it to `index.html`**
4. Visit `https://nicoleslohodateam.com/virtual-staging/`

### Path B — Native WordPress page (so Nicole can edit later)

1. WP Admin → Pages → Add New → Title "Virtual Staging"
2. Permalink: `/virtual-staging/`
3. Switch editor to **Code editor**
4. Paste the entire `<body>` content (everything between `<body>` and `</body>` in `virtual-staging.html`)
5. Move the `<style>` block to **Appearance → Customize → Additional CSS** (prefix every selector with `.page-id-NN` or `body.virtual-staging-page` to scope it to this page only)
6. Move the `<script>` block to a Custom HTML block at the bottom of the page
7. Publish

> Path A is recommended for shipping today. Path B once Nicole wants to edit copy without code.

### Image assets to upload for Virtual Staging

The page references these URLs — all will gracefully fall back to a labeled placeholder if missing. Upload to `/wp-content/uploads/virtual-staging/` (via WP Media Library or File Manager):

| Filename | Position | Notes |
|---|---|---|
| `hero-before.jpg` | Hero slider — left half | The "before" shot for the interactive drag-slider at the top |
| `hero-after.jpg` | Hero slider — right half | Same room/angle as `hero-before.jpg`, staged |
| `vs-living-before.jpg` / `vs-living-after.jpg` | Portfolio card 1 | Living Room example |
| `vs-kitchen-before.jpg` / `vs-kitchen-after.jpg` | Portfolio card 2 | Kitchen remodel example |
| `vs-bedroom-before.jpg` / `vs-bedroom-after.jpg` | Portfolio card 3 | Bedroom example |
| `vs-exterior-before.jpg` / `vs-exterior-after.jpg` | Portfolio card 4 | Exterior example |

Until those exist, the page shows tasteful labeled placeholders — totally fine for Nicole's first review.

## Step 8 — Add a "Virtual Staging" nav link to the homepage

The homepage nav (`index.html`) does NOT yet have a Virtual Staging link. Add it in 30 seconds:

In `index.html`, find the `<ul class="nav-links">` block and insert this `<li>` before the `Contact` item:
```html
<li><a href="/virtual-staging/">Virtual Staging</a></li>
```

Same change in the footer's "Explore" column.

## Known followups (not blocking launch)

1. **Real testimonials** — replace the 3 placeholders with her best Zillow + Google reviews.
2. **Real photography for community cards** — currently a stylized gradient. Drop in town hero images (Metuchen, Edison, Woodbridge, etc.) when she has approved photos.
3. **Pre-approval CTA** — wire to a lender partner she works with.
4. **Blog / Market Updates** — intake left this empty; we can stub a `/blog/` index when she's ready to publish.
5. **SEO meta refinement** — schema.org `RealEstateAgent` JSON-LD block can be added once we lock the final headshot URL.
