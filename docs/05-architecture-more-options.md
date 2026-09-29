# More Architecture Variants (B.2–B.5)

These all share Option B's core shape — static/hybrid front end, headless content,
managed hosting, no servers to babysit — but swap the framework/CMS/hosting choices
to trade off "cool factor," how much it plays to your existing C#/.NET skills, and how
much future custom capability you want to build in. B.1 (the Astro + Sanity plan) stays
the low-effort baseline; these lean into "I'm open to more work for cooler stuff later."

Quick comparison first, details below.

| | Frontend | CMS | Hosting | Vibe |
|---|---|---|---|---|
| **B.1** | Astro | Sanity | Cloudflare Pages/Vercel | Simple, minimal JS, lowest effort |
| **B.2** | Next.js (React) | Sanity | Vercel | Most polished animation/interactivity ecosystem |
| **B.3** | SvelteKit | Sanity or Storyblok | Cloudflare Pages | Lightweight, animation-friendly, less boilerplate than React |
| **B.4** | Astro (marketing) + Blazor WASM (interactive tools) | Sanity or flat files | Azure Static Web Apps | Lets you use C#/.NET directly, "app-like" bonus features |
| **B.5** | Next.js or SvelteKit frontend + your own API | Custom (Django/FastAPI) | Managed PaaS (Render/Railway/Fly.io) | Most power/control, most future capability, most of your own build work |

---

## B.2 — Next.js + Sanity + Vercel (the "React ecosystem" option)

- Same idea as B.1, but on **Next.js** instead of Astro. You get React everywhere,
  which unlocks the biggest ecosystem of polish: **Framer Motion** for smooth
  scroll/entrance animations, easy parallax, animated SVG "light rays," page
  transitions, etc. — good fit for the "subtle light/illumination" visual identity
  the founders described, done really well rather than as static images.
- Next.js API routes give you a place to run small bits of custom server logic
  (e.g., a contact form handler, a future "which pathway is right for my child?"
  quiz that emails results) without standing up a separate backend — still deploys
  as one Vercel project, still no server to patch.
- Slightly heavier than Astro (more JS shipped by default) but Next's static/ISR
  rendering keeps it fast; totally fine for a marketing site.
- **Cool-factor payoff:** best-documented path if you want scroll-triggered
  animations, subtle parallax on the "rays," animated page transitions between the
  three pillars, etc. Huge community/examples to pull from.
- **Trade-off vs B.1:** more moving parts (React state, hooks) for stuff that Astro
  would do with less code, since Astro is content-first and React is app-first. Worth
  it only if you actually want the animation/interactivity ecosystem.

---

## B.3 — SvelteKit + Sanity/Storyblok + Cloudflare Pages (the "lightweight but fun" option)

- **Svelte** compiles your components down to small, plain JS — no virtual DOM
  overhead, less boilerplate than React for the same result. Genuinely enjoyable to
  write, and a good "new skill" to pick up since it maps reasonably close to plain
  HTML/CSS/JS rather than a big framework mental model.
- Built-in transition/animation primitives (`transition:`, `animate:` directives) are
  first-class in Svelte — the "three rays," fade/slide reveals, subtle motion on
  scroll are very natural here, arguably easier to reach for than in React.
- **Storyblok** is worth mentioning as an alternative CMS pairing here: visual,
  component-based editor (founders literally see the page layout while editing) —
  a different "cool" editing experience than Sanity's more form-like Studio. Sanity
  also works fine with SvelteKit if you'd rather keep that choice from B.1.
- Cloudflare Pages has first-class SvelteKit support (adapter built in).
- **Cool-factor payoff:** nicest animation code, smallest shipped JS, a genuinely fun
  framework to learn if you want to expand your front-end skills beyond what you've
  already touched.
- **Trade-off:** smaller ecosystem/community than React — fewer copy-paste examples
  for very specific things, though for a marketing site this rarely matters.

---

## B.4 — Astro/Blazor hybrid on Azure Static Web Apps (the "use your C# skills" option)

This is the one built specifically to flex your .NET background rather than pushing
you further into the JS ecosystem.

- Marketing pages (Home, About, Academic Therapy, Resources, Professional Learning,
  Contact) stay simple: Astro (or even a .NET static site generator like **Statiq**)
  pulling from Sanity or flat content files — same low-maintenance content story as B.1.
- For anything genuinely "app-like" you want to add later — e.g., an interactive
  **literacy/dyslexia screening quiz**, a **"which service is right for my child"**
  interactive tool, a future **client portal** for academic therapy progress, or a
  **course-progress mini-app** for Professional Learning — you build that piece as a
  **Blazor WebAssembly** component, in C#, mounted at its own route (e.g., `/screener`)
  within the same site.
- **Azure Static Web Apps** hosts this well: it natively supports a static front end
  plus optional **Azure Functions written in C#** for any server-side logic (form
  processing, sending quiz results by email, storing intake submissions) — all still
  managed/serverless, free tier is generous, no VM/server to patch.
- **Cool-factor payoff:** this is the option where "cool future things" plausibly
  means real applications (screeners, client portals, progress-tracking tools) built
  in a language you already know well, not just visual flourishes.
- **Trade-off:** two different tech stacks living in one project (Astro/JS for
  marketing pages, Blazor/C# for interactive tools) — more architectural surface area
  than B.1–B.3, though each piece individually stays simple and managed.

---

## B.5 — Custom backend + modern frontend (the "most control, most future capability" option)

For a lot more optionality later, at the cost of owning more of the system:

- Frontend: Next.js or SvelteKit (whichever you preferred from B.2/B.3), still
  statically deployed on Vercel/Cloudflare Pages.
- Backend: a real API you write and own — **Django (Python)** or **ASP.NET Core
  Web API (C#)** — instead of a third-party headless CMS. This is genuinely useful
  if you want to eventually build things a CMS+embeds can't easily do: a custom
  student/client portal with logins, a real digital storefront with your own
  licensing logic for resources, a custom course-delivery platform for Professional
  Learning rather than linking out to Teachable, structured intake workflows tied
  into your own database.
- Hosting the backend on a **managed PaaS** (Render, Railway, Fly.io, or Azure App
  Service) rather than a raw VM keeps this from becoming "real server maintenance" —
  you don't patch the OS, but you are responsible for app-level upgrades, database
  backups/migrations, and it's simply more surface area than the other options.
- **Cool-factor/future payoff:** the most future capability of any option here — if
  the business really grows (its own LMS, its own client portal, custom licensing/
  sales logic, integrations you fully control), this is the option that scales into
  that without hitting a ceiling.
- **Trade-off:** this is the one place where "low maintenance" and "cool/powerful"
  genuinely pull in different directions. It's a legitimate choice *if* you expect to
  want real custom backend features within a year or two — otherwise it's premature,
  since B.1/B.4 can already reach most "cool" goals via animation + a Blazor tool or
  two without owning a database/API in production.

---

## How to think about picking among these
- If "cool" mostly means **visual polish and delightful motion** on an otherwise
  content-driven site → **B.2** (React/Framer Motion) or **B.3** (Svelte, lighter and
  arguably more fun to build).
- If "cool" mostly means **interactive tools/mini-apps** (quizzes, screeners, a future
  client portal) and you want to use **C#** for that → **B.4**.
- If "cool" means **owning a real product platform** eventually (custom LMS, custom
  storefront, logins/accounts) and you're fine with more ongoing responsibility →
  **B.5**, but treat it as a "later, if needed" upgrade path rather than the starting
  point.
- **B.1 remains the safe, fully-capable default** — nothing above is required to hit
  the founders' actual V1 requirements; these are about how much you personally want
  to build and learn along the way.
