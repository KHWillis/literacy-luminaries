# Tech Stack Options

Two audiences need to be happy with the stack:
1. **You** — low ongoing maintenance, nothing you need to babysit (security patches,
   server upkeep, breaking dependency upgrades).
2. **The founders** — need to edit text/images/bios themselves eventually without
   filing a ticket with you, especially as Resources/Professional Learning pages grow.

Below are three realistic paths, roughly ordered from "least code, least control" to
"most code, most control."

## Option A — No-code / low-code website builder
**Examples:** Squarespace, Webflow, Wix Studio

- ✅ Founders can edit almost everything themselves (text, images, layout blocks) with
  no dev involvement.
- ✅ Hosting, SSL, backups, uptime, security patching all handled by the vendor — zero
  maintenance for you.
- ✅ Built-in support for future needs: online scheduling (via embeds/integrations),
  payments, digital downloads (Squarespace has native digital products; Webflow via
  Foxy/Memberstack/Gumroad embeds), email signup forms/newsletters, blog.
  Squarespace in particular is strong here for a small service business.
- ✅ Professional design templates exist that fit "sophisticated, warm, modern" without
  design-from-scratch effort.
- ❌ Monthly subscription cost ($16–$40/mo range depending on plan/features).
  Not so much a downside since a for-profit business should expect operating costs.
- ❌ Less flexible for fully custom interactions; you're working within the builder's
  design system.
- ❌ Doesn't flex your C#/Python skills — this is mostly a design/content job, not
  a coding job, for you.

**Best if:** the top priority is "the founders can run this with zero developer
involvement in a year," and you're fine acting as designer/consultant rather than
engineer.

## Option B — Static site generator + headless CMS, on managed hosting
**Examples:** Astro or Next.js (static export) + a headless CMS (Sanity, Contentful,
or even a simple Git-based CMS like Decap/TinaCMS) + Vercel/Netlify/Cloudflare Pages hosting.

- ✅ You write real code (front-end, some TypeScript/JS — reasonable stretch from your
  background), full control over design/motifs (the "three rays," subtle illumination
  imagery, etc.) without fighting a builder's constraints.
- ✅ Hosting on Vercel/Netlify/Cloudflare Pages is effectively maintenance-free: git push
  → auto build/deploy, free SSL, CDN, generous free tiers for a small business site.
  No servers to patch.
- ✅ Headless CMS gives founders a simple editing UI (like a form) for bios, resource
  listings, blog posts — without touching code.
- ✅ Very cheap to run (often $0–$20/mo total: hosting free tier + CMS free/low tier +
  domain).
- ➖ You are the one who builds and later updates the front-end code for structural
  changes (new page types, new sections) — founders can't add a whole new "Shop" section
  by themselves, that still needs a developer pass. But day-to-day content is self-serve.
- ➖ More upfront build time than a no-code builder.
- ✅ Payments/scheduling later: bolt-on with hosted embeds (Calendly for scheduling,
  Stripe Payment Links or Stripe Checkout for digital resource sales, Substack/Buttondown/
  Mailchimp for newsletter) — no need to build these yourself.

**Best if:** you want to actually build it, want full design control, and are OK being
the one who does structural/feature changes down the road (which sounds like the plan,
since you're the "technical side" person).

## Option C — Full custom app (e.g., ASP.NET Core / Django backend + database)
- ✅ Maximum flexibility, plays directly to your C#/.NET or Python strengths.
- ❌ Means you own a running server/app: hosting, patching the framework, database
  backups, uptime, scaling, security — real ongoing maintenance burden.
- ❌ Massive overkill for a marketing site + eventual light e-commerce/scheduling. Nearly
  everything the business needs (payments, scheduling, digital sales, email) already
  exists as a reliable managed service — building it yourself is extra risk for no
  benefit here.

**Not recommended** given the explicit "low maintenance for me" goal, unless there's a
specific reason to want a custom backend later (e.g., a proprietary curriculum-delivery
platform down the road — and even then, that could be a *separate* app added later,
not the reason to complexify the marketing site now).

## Recommendation
Lean toward **Option B** (Astro + a lightweight headless/Git-based CMS, deployed on
Vercel or Cloudflare Pages) as the sweet spot:
- Practically zero server maintenance (static hosting + managed CMS).
- You get to build something real and keep full control of the distinctive visual
  identity (the light/illumination motifs, three-part visual theme) that a template-driven
  builder might constrain.
- Future features (payments, scheduling, email) attach as hosted third-party services,
  not things you build/maintain.
- Cheapest ongoing run cost.

**Option A (Squarespace/Webflow)** is a perfectly reasonable fallback if, after scoping
the build, you'd rather not spend design/build time and prioritize the founders'
total independence from you — worth a real discussion, not just a footnote.

Either way — **avoid Option C** style server-hosted custom app for the marketing site.
