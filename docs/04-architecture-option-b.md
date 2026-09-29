# Option B Deep Dive — Static Site + Headless CMS

This fleshes out Option B into a concrete, buildable architecture. Each section is a
decision point: recommendation first, alternatives after, so you can swap pieces out
without redoing the whole plan.

---

## 1. Framework — **Astro** (recommended)

- Ships **zero JS by default**; you only add interactivity (React/Vue/Svelte "islands")
  where actually needed (e.g., a mobile nav toggle, a contact form). Great fit for a
  mostly-content marketing site — fast, simple, less to maintain than a full SPA framework.
- File-based routing, plain `.astro` components (HTML + minimal templating syntax) —
  a gentle ramp from your C#/Python background, not a steep "learn React deeply" curve.
- Can still drop in a React component later if you want something more interactive
  (e.g., an intake form wizard, a resource filter).
- Built-in support for Markdown/MDX and "content collections" (schema-validated content,
  which pairs nicely with a CMS or even just local files).

**Alternative:** Next.js (static export) — more powerful/flexible, bigger ecosystem, but
heavier and more opinionated than this project needs. Would make sense if the site were
going to become a full web app; it isn't.

---

## 2. Content management — **Sanity** (recommended)

For a non-technical editor (founders), the CMS editing *experience* matters more than
which one is technically "best." Two real options:

### Sanity (hosted headless CMS)
- ✅ Real form-based editing UI ("Sanity Studio") — text fields, rich text, image
  upload with built-in cropping/hotspot, no git/markdown knowledge required.
- ✅ Structured content: define schemas for "Founder Bio," "Resource," "Service Page,"
  etc. — this directly supports the "model Resources/Professional Learning as
  expandable collections now" goal from the roadmap.
- ✅ Generous free tier (plenty for a small business site: seats, API requests,
  bandwidth all well above what this needs).
- ✅ Built-in image CDN (auto-resizing/optimization) — one less thing to build.
- ✅ Studio can be deployed for free (e.g., `studio.literacyluminaries.com`) so founders
  just log into a website — no local software, no git.
- ➖ It's a separate hosted service/account (still: you're not the one operating it,
  Sanity is — consistent with "low maintenance for me").

### Git-based CMS (Decap CMS / TinaCMS)
- Content stored as Markdown/YAML files in the same git repo as the site.
- ✅ No third-party service dependency beyond git hosting; content is just files.
- ➖ Editing UI is more basic; image handling/rich content is clunkier.
- ➖ Auth setup (e.g., Decap + Netlify Identity/GitHub) adds a bit of moving-parts
  complexity for not much payoff at this scale.
- Better fit for a developer-only content workflow, less ideal for a fully non-technical
  editor doing things solo.

**Recommendation: Sanity.** The founders will eventually want to add resources, maybe
blog posts, PD offerings — a proper structured-content editing UI pays off, and the
free tier removes the cost objection.

---

## 3. Hosting/deploy — **Cloudflare Pages or Vercel** (either is fine)

- Connect the git repo (GitHub) → every push to `main` auto-builds and deploys. No
  server to patch, no uptime to babysit.
- Free SSL, global CDN, generous free tier for a small business site.
- Preview deployments per branch/PR — useful if you want to show the founders a draft
  before it goes live.
- **Cloudflare Pages**: slightly more generous free tier, pairs well if you also want
  Cloudflare for DNS.
- **Vercel**: extremely smooth DX, best-in-class for Astro/Next, also a fine choice.
- Either way: domain DNS can point here directly; no separate "server" concept at all.

---

## 4. Content model (maps directly to the brief)

Set these up as Sanity schemas / Astro content collections:

- `founder` (repeatable, 3 initially): name, credentials, photo, short bio, personal note
- `servicePillar` (repeatable, 3: Academic Therapy / Literacy Resources / Professional
  Learning): title, one-line summary (from homepage copy in the brief), status
  (`live` / `coming soon`), long-form page content
- `resource` (repeatable, empty at launch): title, description, price (later), type
  (curriculum / download / course), status (draft/coming soon/published) — modeling this
  now means "Resources" page can go from "coming soon" to a real catalog without a
  rebuild, just new entries + a listing template
- `page` (generic): About Us, Contact — simpler flat content
- `siteSettings` (singleton): tagline, contact email, social links, color/brand tokens if
  you want them editable

This directly satisfies the roadmap goal: Phase 1 ships with `resource` and
`servicePillar` types that say "coming soon," Phase 2/3 just add real entries.

---

## 5. Forms, email, newsletter (all bolt-on services, not custom-built)

- **Contact form:** a lightweight form-handling service (e.g., Formspree, or Cloudflare
  Pages Functions + Resend if you want a bit more control) — avoids needing your own
  backend/mail server.
- **Newsletter signup:** embed a signup form from Buttondown, Mailchimp, or ConvertKit.
  Pick this later once the founders know if they want a real email marketing tool or
  just an interest list.
- **Future scheduling:** Calendly/Acuity embed on the Contact or Academic Therapy page.
- **Future payments:** Stripe Payment Links / Checkout for digital resources; a course
  platform (Teachable/Thinkific) for Professional Learning courses if that grows —
  linked from the site rather than built into it.

---

## 6. Styling — Tailwind CSS (recommended)

- Utility-first CSS pairs naturally with Astro components, keeps the "sophisticated,
  warm, modern" brand consistent via a small design-token setup (burgundy/pink/black/
  gray/white scale + spacing/typography scale) rather than one-off CSS per page.
- Good accessibility defaults are easy to enforce (focus states, contrast) since you
  control the token values directly.

---

## 7. Repo & dev workflow

```
literacy-luminaries-site/
  src/
    components/       # shared UI (nav, footer, buttons, three-rays motif, etc.)
    layouts/
    pages/             # routes: index, about, academic-therapy, resources, ...
    content/           # Astro content collections (typed, can sync from Sanity)
    styles/
  public/              # static assets, favicon, etc.
  astro.config.mjs
  sanity/              # Sanity Studio config (schemas), can live in same repo or a
                        # sibling repo — same-repo is simpler for a solo maintainer
```

- GitHub repo → Cloudflare Pages/Vercel auto-deploy on push to `main`.
- PR/preview branches for anything you want to sanity-check before going live.
- Local dev: `astro dev` for the site, `sanity dev` for the Studio — both are just
  `npm` commands, no VM/server setup.

---

## 8. Non-functional stuff worth baking in from day one
- **Accessibility:** WCAG 2.1 AA baseline (semantic HTML, alt text fields required in
  CMS schemas, color-contrast-checked palette, keyboard-navigable nav). Especially
  on-brand given the subject matter.
- **SEO:** per-page title/meta description fields in the CMS, sitemap.xml + robots.txt
  (Astro has plugins for this), Open Graph image per page for social sharing.
- **Analytics:** privacy-friendly option (Plausible or Cloudflare Web Analytics) unless
  there's a specific reason to want full Google Analytics.

---

## 9. Rough cost estimate (V1)

| Item | Cost |
|---|---|
| Domain | ~$12–20/yr |
| Hosting (Cloudflare Pages/Vercel free tier) | $0/mo |
| Sanity (free tier) | $0/mo |
| Form handling (free tier) | $0/mo |
| Email/newsletter (free tier up to a subscriber threshold) | $0/mo |
| **Total** | **~$15/yr** until the business is big enough to outgrow free tiers |

This is the practical payoff of Option B for your "low maintenance" goal: near-zero
recurring cost *and* near-zero operational burden, while still being a real,
fully-custom-designed site.

---

## Open decisions to make before starting the build
- [ ] Cloudflare Pages vs. Vercel (either is fine — pick one, maybe based on whether
      you want Cloudflare handling DNS too)
- [ ] Confirm Sanity as CMS vs. going simpler with Decap/Tina if you'd rather avoid a
      third-party account entirely
- [ ] Decide now whether Sanity Studio lives in the same repo as the site or a separate
      one (recommend: same repo, simpler for a solo dev)
