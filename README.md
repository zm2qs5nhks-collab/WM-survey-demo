# World Models for Physical AI — survey site

Landing page for the survey **World Models for Physical AI: Learning to Predict, Plan, and Act
in the Real World**.

**Live page:** <https://zm2qs5nhks-collab.github.io/WM-survey-demo/>

One static page. No build step, no framework, no dependencies — what is in this repository is
exactly what the browser receives.

---

## What the page does

The page is an index of the survey's argument, not a condensed version of it. Each section makes
one claim and is anchored by one figure taken from the manuscript:

| Section | Anchor | Claim it makes |
|---|---|---|
| Abstract | — | The thesis, the three gaps, and how the survey is organised (Fig. 1) |
| Research landscape | Fig. 2 | Six directions, grouped by consumer rather than architecture |
| Where prediction enters | Figs. 3–6 | Three interfaces, three different quantities measured |
| Evidence | Table 7 | The contrasts behind the claims, and what remains unrun |
| Open problems | — | Four gaps the survey cannot settle |
| Citation | — | BibTeX |

The three cross-study findings appear once, as the card row under the abstract, and are then
*elaborated* by figures rather than restated. An earlier draft repeated them in a separate Findings
section and again in a Synthesis row, which is what made the page read as long.

**Figure numbering matches the manuscript 1:1.** Page Figures 1–6 are the manuscript's Figures 1–6,
and the only table on the page is the manuscript's Table 7 (labelled Table 1 here). The manuscript's
Figure 4 was missing from an earlier PDF revision and is now included; if a future revision drops or
renumbers a figure, update the `<figcaption>` labels so this correspondence stays true.

## Repository layout

```
index.html            the entire page
assets/style.css      all styling; design tokens are at the top of the file
assets/*.webp         manuscript figures, converted from the source docx
assets/table7.png     screenshot of the manuscript's Table 7
assets/og-cover.png   1200x630 social card (absolute URL in the meta tags)
robots.txt
```

Every file in `assets/` is referenced by `index.html`. Unused prototypes and earlier placeholder
SVGs were removed; they remain in git history if needed
(`git checkout <commit> -- assets/<file>`).

## Custom domain

There is no `CNAME` file, because none is configured: the site is served from the default
`*.github.io` address. To attach a domain:

1. Create `CNAME` containing one line — the bare domain, no scheme, no trailing slash:
   ```
   survey.example.com
   ```
   Comments are not allowed in this file; a placeholder value will be rejected by Pages.
2. In the DNS provider, point the apex at GitHub's four `A` records, or point a subdomain at
   `<user>.github.io` with a `CNAME` record.
3. Repository → Settings → Pages → Custom domain, enter the same value, then wait for the
   certificate before enabling **Enforce HTTPS**.

## Editing conventions

Keep to these or the page will drift out of shape:

- **One section, one wrapper.** Every section is `<section><div class="wrap">…</div></section>`.
  Inside `.wrap`, use only these children: `.sec-head`, `.prose`, `.grid.g2` / `.grid.g3`,
  `figure`, `.note`.
- **No inline styles.** Vertical rhythm comes from a single rule,
  `section .wrap > * + * { margin-top: 30px }`. Adding `style="margin-top:…"` breaks the rhythm.
- **One background colour.** `--bg-page` is used by `body`, `.hero` and `.nav`. Giving the hero
  its own background paints a second colour into the same viewport and produces a visible seam at
  the first section boundary.
- **Tables are images.** HTML tables were removed because they cannot be trusted to stay aligned
  when re-typeset outside the original column geometry. Use a screenshot with a written caption.
- **Every figure needs a caption.** The caption carries the caveat the figure itself cannot state
  (what the protocol was, what the schematic omits, what the result does not settle).

## Replacing a figure

1. Export at roughly 2× the display size, maximum width 1700 px.
2. Convert to WebP to keep the page light:
   ```bash
   python -c "from PIL import Image; im=Image.open('in.png').convert('RGB'); im.save('assets/fig-x.webp','WEBP',quality=86,method=6)"
   ```
3. Update three attributes on the `<img>`: `src`, `width`, `height`. The size attributes reserve
   space and prevent layout shift while the image loads.
4. Rewrite the `<figcaption>` if the figure's scope changed.

## Deployment

GitHub Pages serves this repository from `main`, root folder. Push and the site rebuilds in about
a minute:

```bash
git add -A
git commit -m "…"
git push origin main
```

To serve locally without any tooling:

```bash
python -m http.server 8000
```

## Figures and numbers

- Figures 1, 2, 3, 5 and 6 are the manuscript's own diagrams, extracted from the source docx.
  Figure 4 is not embedded in that file and is therefore not reproduced.
- Table 7 is a screenshot of the manuscript table, not a re-typeset copy.
- **Every number on the page is a within-study anchor.** It belongs to one study's protocol and
  baseline; figures from different studies are not comparable, and the page says so where the
  numbers begin.

## Citation

See the BibTeX block at the bottom of the page. Author information is withheld while the paper is
under review; update that block and the author line once it is public.
