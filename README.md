# QGIS Advanced Training — LRBI Resource Portal

A clean, GIS-inspired, single-page resource portal for the **QGIS Advanced Training for Local Roads and Bridges Inventory (LRBI)**.

The page is a single self-contained `index.html` file: embedded CSS, embedded JavaScript, inline SVG icons and cartographic artwork. It makes **zero network requests** (no external fonts, stylesheets, scripts, or images), so it works reliably inside a Google Sites iframe.

---

## 1. Files

| File | Purpose |
|---|---|
| `index.html` | The complete portal. Host this file (or its URL) and embed it in Google Sites. |
| `README.md` | This guide — editing links, customizing content, and embedding instructions. |

---

## 2. Quick start

1. Open `index.html` in any modern browser to preview it locally.
2. Edit the `TRAINING_CONFIG` object near the bottom of `index.html` (see [Section 3](#3-where-to-replace-the-google-sites-page-urls)).
3. Upload `index.html` to a static host.
4. Embed the public HTTPS URL in Google Sites (see [Section 6](#6-embedding-in-google-sites)).

No build step, package manager, server, or backend is required.

---

## 3. Where to replace the Google Sites page URLs

Open `index.html` and scroll to the bottom of the file. Look for this clearly marked block:

```js
/* ========================================================================
   REPLACE YOUR GOOGLE SITES URLS HERE
   ------------------------------------------------------------------------
   This is the ONLY place you need to edit links.
   ======================================================================== */
const TRAINING_CONFIG = {
  /* ---- Google Sites page links (EDIT THESE THREE) -------------------- */
  dataUrl: "INSERT_DATA_PAGE_URL",
  presentationsUrl: "INSERT_PRESENTATIONS_PAGE_URL",
  assetsUrl: "INSERT_ASSETS_PAGE_URL",
  ...
};
```

Replace the three placeholder values with the real Google Sites page URLs:

| Config key | Portal card | Button |
|---|---|---|
| `dataUrl` | **Data** | EXPLORE DATA |
| `presentationsUrl` | **Presentations** | VIEW PRESENTATIONS |
| `assetsUrl` | **Assets** | ACCESS ASSETS |

Example:

```js
dataUrl: "https://sites.google.com/view/lrbi-training/data",
presentationsUrl: "https://sites.google.com/view/lrbi-training/presentations",
assetsUrl: "https://sites.google.com/view/lrbi-training/assets",
```

### Getting a Google Sites page URL

1. Open the site in Google Sites and click **Publish** (or **Republish**).
2. In the published site, navigate to the page you want to link.
3. Copy the full URL from the browser address bar (`https://sites.google.com/...`).
4. Paste it into the matching key in `TRAINING_CONFIG`.

### How unfinished links behave

Any value that is empty, `#`, or still contains `INSERT_...` is treated as **not configured**. Clicking that card or button does not navigate; instead a small notice appears:

> Data library — set its URL in TRAINING_CONFIG (index.html).

The affected card also gets a dashed border as a visual reminder. Once a real URL is entered, the notice and dashed border disappear automatically.

> **Important:** Do not edit the `href` attributes in the HTML markup. All navigation is applied from `TRAINING_CONFIG` when the page loads, so changing the config is sufficient.

---

## 4. Editing other links and content

Everything editable lives in the same `TRAINING_CONFIG` object:

### Quick access links

```js
qgisDownloadUrl: "https://qgis.org/download/",
documentationUrl: "https://docs.qgis.org/",
scheduleUrl: "#",
exercisesUrl: "#",
```

Replace `scheduleUrl` and `exercisesUrl` with your own Google Sites pages or shared document links. The labels and descriptions for these shortcuts are defined in `quickLinks`:

```js
quickLinks: [
  { linkKey: "qgisDownloadUrl", label: "Download QGIS", note: "Install the latest release", icon: "download" },
  ...
]
```

`linkKey` must match a URL key in the config. Available icon names: `download`, `book`, `calendar`, `clipboard`, `grid`, `layers`, `database`, `map`, `pin`, `file`, `route`, `compass`, `crosshair`, `activity`, `contour`, `package`, `presentation`.

### Training modules

Add, remove, or reorder modules by editing the `modules` array:

```js
modules: [
  {
    title: "Local Roads and Bridges Inventory and GIS Fundamentals",
    description: "Introduction to LRBI, coordinate systems, the QGIS interface, and core geospatial concepts.",
    icon: "compass",
    url: "#"          // optional: link to a Google Sites module page
  },
  ...
]
```

- Module numbers are generated automatically in Roman numerals (I, II, III, IV, V...). Set `number: "I-A"` to override.
- Add `url: "https://sites.google.com/..."` to make a whole module card clickable.
- The header counters update automatically.

### Attribution / institutions

Edit the `partners` array to replace the logo placeholders and names:

```js
partners: [
  { initials: "DILG", name: "Organizing Institution", role: "Lead agency" },
  { initials: "LGU",  name: "Partner Agency",        role: "Local government" },
  { initials: "GIS",  name: "Technical Support",     role: "Training team" }
]
```

`initials` is the placeholder monogram shown inside the dashed square. Replace it with a short abbreviation, or extend the CSS to use real images if desired (not recommended for the self-contained requirement).

### Link behaviour

```js
openInNewTab: true
```

- `true` (default): resource links open in a new browser tab with `rel="noopener noreferrer"`. This is recommended inside Google Sites because it keeps the embedded portal visible and avoids awkward nested navigation.
- `false`: links navigate in the same frame.

### Visible text

Hero title, subtitle, supporting copy, card descriptions, section headings, and footer text are plain HTML. Use your editor's search function to find and replace them directly in `index.html` (search for "QGIS", "Your central hub", or a card description).

---

## 5. Customizing the design

All colours, spacing, radii and fonts are CSS custom properties at the top of the `<style>` block:

```css
:root {
  --green-800: #164B3B;   /* primary deep forest green */
  --accent-qgis: #589632; /* QGIS green */
  --accent-teal: #2C8C85; /* teal accent */
  --bg: #F7F9F7;          /* off-white background */
  --ink: #263238;         /* dark charcoal text */
  --sage: #E6EFE8;        /* light sage */
  --font: "Inter", "Roboto", ...;
}
```

Change these values to re-theme the entire portal. Typography uses Inter or Roboto when installed, with a system-font fallback so no font download is needed. To force-load a web font, add a Google Fonts `<link>` in `<head>` — but note that this adds an external request.

The map illustration, contour lines, coordinate grid, and icons are inline SVG. They can be edited in place; no image files are involved.

---

## 6. Embedding in Google Sites

Google Sites' **Embed Code** option accepts a **public HTTPS URL**. A local file path (`file:///...`) or a private/intranet URL will not work for visitors.

### Step 1 — Host the file publicly

Use any static host, for example:

- GitHub Pages
- Netlify Drop / Netlify Sites
- Vercel
- Firebase Hosting
- Your institution's static web server

Upload `index.html` as `index.html` (or rename it and use the full file URL). Confirm the page loads over **https://** in a normal browser tab.

### Step 2 — Embed in Google Sites

1. Open your Google Site in **Edit** mode.
2. Place the cursor where the portal should appear.
3. Choose **Insert → Embed Code** (or the `</>` icon in the toolbar).
4. Paste the public URL of the hosted page, e.g. `https://your-site.example/lrbi-portal/`.
5. Click **Insert**, then **Publish**.
6. Drag the bottom-right handle of the embedded frame to a comfortable height (roughly 1100–1400 px on desktop is a good starting point). The page is fluid — it has no fixed height and no nested scrolling.

### Step 3 — Check the result

- View the published site while signed out (or in an incognito window) to confirm public access.
- Test on a phone: the portal collapses to one column and remains readable.
- Click each of the three cards and the quick-access links.

### Google Sites compatibility notes

- The page uses fluid widths and natural document height, so it does not create a second scrollbar inside the frame.
- No cookies, authentication, pop-ups on load, or browser-restricted APIs are used.
- No external assets means no mixed-content or CORS problems.
- If the embed shows a blank frame, check that the host allows framing (`X-Frame-Options`/CSP `frame-ancestors` must not block `sites.google.com`). GitHub Pages and Netlify work by default.
- If links seem unresponsive, confirm you replaced the `INSERT_...` placeholders rather than editing only the visible button text.

---

## 7. Quality assurance checklist

Use this list after editing:

- [ ] All three `INSERT_...` Google Sites URLs replaced.
- [ ] `scheduleUrl` and `exercisesUrl` set, or intentionally left as `#`.
- [ ] Desktop (≥1200 px), tablet (~768 px) and mobile (~390 px) views checked.
- [ ] No horizontal scrolling at any width.
- [ ] Cards, buttons and shortcuts are keyboard-focusable with a visible focus ring.
- [ ] External links open in a new tab (or same frame if `openInNewTab` is `false`).
- [ ] All icons render (no missing SVG symbols).
- [ ] Text remains readable inside the Google Sites iframe at its chosen height.
- [ ] Page loads instantly with no network requests in DevTools.
- [ ] No backend, build step, or local asset requirement.
- [ ] `prefers-reduced-motion` users see a static, fully visible page.

---

## 8. Troubleshooting

| Symptom | Cause / fix |
|---|---|
| Clicking a card shows a notice | Its URL is still a placeholder. Edit `TRAINING_CONFIG`. |
| Card link works locally but not in Google Sites | The URL is a local path, or the target page is not published/public. |
| Blank iframe | Host blocks framing, or the URL is HTTP while the site is HTTPS. Use HTTPS and a host that permits embedding. |
| Two scrollbars | Increase the iframe height in Google Sites; the page itself uses natural height. |
| Icons missing | Do not remove the hidden `<svg>` with `<symbol>` definitions near the top of `<body>`. |
| Font looks different | Inter/Roboto are not installed on that device; the system fallback is intentional. |

---

## 9. Design summary

- **Visual identity:** GIS dashboard with contour lines, coordinate grids, road-network strokes, map pins/nodes, layer stacks and lat/long labels.
- **Palette:** deep forest green `#164B3B`, QGIS green `#589632`, teal `#2C8C85`, off-white `#F7F9F7`, charcoal `#263238`, light sage `#E6EFE8`.
- **Hierarchy:** three prominent resource cards (Data, Presentations, Assets) supported by a four-module pathway and four quick shortcuts.
- **Accessibility:** semantic landmarks, skip link, `aria-label`s, visible focus states, reduced-motion support, high-contrast text.
- **Performance:** single HTML file, inline SVG, no dependencies, no network requests.
