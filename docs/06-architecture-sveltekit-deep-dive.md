# SvelteKit Deep Dive (Option B.3)

You liked B.3 for the "fun to learn" factor — this fleshes it out the same way B.1 was
fleshed out, so it's an equally real, buildable plan rather than just a paragraph.

---

## 1. Why SvelteKit specifically (and what "fun" looks like in practice)

- Svelte is a **compiler**, not a runtime framework like React/Vue. You write
  components that look almost like plain HTML/CSS/JS, and Svelte compiles them into
  small, efficient vanilla JS at build time — no virtual DOM, very little boilerplate.
- Reactivity is refreshingly simple: assigning to a variable (`count += 1`) just
  updates the UI. No `useState`, no dependency arrays, no hooks to remember.
- **Animation is a first-class citizen** — this is the big "fun" payoff for a site
  built around light/illumination/growth motifs:
  - `transition:fade`, `transition:fly`, `transition:slide` — built-in, one-line
    element transitions (e.g., the three pathways fading/sliding into view on scroll).
  - `animate:flip` — smooth reordering animations (e.g., resource cards reflowing).
  - Custom easing/spring physics via `svelte/motion` (`tweened`, `spring` stores) —
    genuinely nice for something like an animated "ray of light" sweeping across the
    hero, or a subtle glow/pulse on hover, without reaching for a heavy animation
    library.
  - Scroll-triggered reveals are simple to hand-roll with Svelte's reactivity + the
    IntersectionObserver API — a good, approachable "first cool feature" to build.
- SvelteKit (the app framework built on Svelte) gives you file-based routing, layouts,
  server-side rendering/static generation, and API routes — the same shape as
  Astro/Next, just in Svelte.

---

## 2. Rendering mode — static-first, same low-maintenance story as B.1

- SvelteKit supports **prerendering** (fully static output) via
  `@sveltejs/adapter-static` or the Cloudflare adapter's static mode — so this stays
  a static site under the hood: no server to run, deploys as files to a CDN.
- If you ever want a server-rendered piece (e.g., a dynamic form endpoint), SvelteKit
  supports that too via its adapters — but for this project, static output is the
  right default, consistent with the "low maintenance" goal.

---

## 3. CMS pairing

Both options from B.1's reasoning still apply here — SvelteKit doesn't change the CMS
decision, just the frontend consuming it:

- **Sanity** (same recommendation as B.1) — form-based editing UI, structured schemas
  for `founder`, `servicePillar`, `resource`, etc. Fetch via Sanity's client inside
  SvelteKit's `load` functions at build time.
- **Storyblok** (new option worth naming here) — a **visual, component-based** editor:
  founders see something closer to the actual page layout while editing, rather than
  a form. Some people find this much more intuitive than Sanity's field-based Studio.
  Free tier is workable for a small site. Slightly more setup work to define
  "bloks" (components) that map to your Svelte components.
- **Recommendation:** start with **Sanity** for consistency with the rest of the
  planning docs (schemas already sketched in `04-architecture-option-b.md` carry over
  directly), but Storyblok is a legitimate swap if the visual editing experience
  matters more to the founders than to you.

---

## 4. Hosting — Cloudflare Pages (recommended pairing)

- SvelteKit has a first-class `@sveltejs/adapter-cloudflare` — build/deploy is
  basically "connect the GitHub repo, push to `main`."
- Free SSL, CDN, generous free tier, same zero-server-maintenance story as everywhere
  else in this plan.
- Vercel also has a solid SvelteKit adapter if you'd rather standardize on Vercel
  across projects — either is fine, Cloudflare is just a natural default if you also
  want them handling DNS for the domain.

---

## 5. Styling

- **Tailwind CSS** still recommended (same token-driven burgundy/pink/black/gray/white
  system as B.1) — works identically well with Svelte components.
- Alternative: Svelte's built-in **scoped `<style>` blocks per component** are good
  enough on their own that some people skip Tailwind entirely in Svelte projects.
  Worth trying both early on to see which you enjoy more — this is one of the more
  "matter of taste" decisions in the whole stack.

---

## 6. A learning-friendly build order (since "fun learning" is the point)

A path that builds real momentum instead of front-loading the hardest concepts:

1. **Static shell first:** build the site with placeholder/local content (no CMS yet)
   — layouts, nav, the six V1 pages, Tailwind tokens for the palette. This is the
   "plain HTML/CSS with superpowers" phase, closest to what you already know.
2. **Add your first `transition:` directives:** fade/slide-in on the three pathway
   cards, a subtle hover state on nav links. Quick wins, immediate visual payoff.
3. **Wire up Sanity:** swap placeholder content for real CMS-fetched content via
   SvelteKit `load` functions. This is where the "content model" from `04` becomes
   real.
4. **Build one genuinely fun interactive piece:** a good candidate given the brand —
   an animated SVG "light rays" hero effect using `svelte/motion` springs, reacting
   subtly to scroll or mouse position. Contained, visual, a nice showcase piece.
5. **Add the contact form + newsletter embed** (same third-party services as B.1:
   Formspree/Resend, Buttondown/Mailchimp).
6. **Polish pass:** accessibility check (reduced-motion support is easy to add and
   important given how animation-heavy this stack choice is — respect
   `prefers-reduced-motion`), SEO meta, sitemap, analytics.

This order means you have a deployable, real site after step 1, and each following
step layers in one new skill/concept rather than everything at once.

---

## 7. Repo shape

```
literacy-luminaries-site/
  src/
    lib/
      components/       # Nav, Footer, PathwayCard, LightRays, etc.
      motion/            # shared spring/tween helpers
    routes/
      +layout.svelte
      +page.svelte              # Home
      about/+page.svelte
      academic-therapy/+page.svelte
      resources/+page.svelte
      professional-learning/+page.svelte
      contact/+page.svelte
    app.css                # Tailwind entry
  static/                 # favicon, static assets
  svelte.config.js
  tailwind.config.js
  sanity/                 # Studio config/schemas (same as B.1)
```

---

## 8. Cost — identical to B.1

Same free-tier stack (Cloudflare Pages + Sanity free tier + free-tier form/email
tools), so still roughly **~$15/yr**, just the domain. Swapping the frontend framework
doesn't change the hosting economics here.

---

## 9. Trade-offs to go in with eyes open

- Smaller ecosystem than React — when you search for "how do I do X in Svelte,"
  you'll find fewer results than for React, though for the scope of this site that
  rarely bites.
- You're learning a new framework, so the first couple of weeks will be slower than
  reusing something you already know — worth it here since "fun learning" was an
  explicit goal, but flagging it as a real (if small) cost against a launch timeline.
- If a task ever needs a very specific third-party React component with no Svelte
  equivalent, you'd have to reimplement it — rare at marketing-site scope, but a
  theoretical gap versus B.2.

---

## Open decisions
- [ ] Sanity vs. Storyblok for CMS (recommend Sanity for continuity with the rest of
      the plan; Storyblok if visual editing matters more)
- [ ] Cloudflare Pages vs. Vercel for hosting (recommend Cloudflare)
- [ ] Tailwind vs. Svelte's native scoped styles (try both early, pick what's more fun)
