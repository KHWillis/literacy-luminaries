# "Light Rays" Hero — Code Sketch

A first cool, contained feature to build once the static shell exists (step 4 in the
learning-friendly build order from `06-architecture-sveltekit-deep-dive.md`). This is
a sketch to build intuition, not final production code — variable names, easing, and
visual details are all up for tweaking once you're looking at it in the browser.

## Concept
A handful of soft, translucent "rays" (three of them, tying back to the three
founders) fan out from behind the hero headline. They drift very slowly on their own
(so the page feels alive even if no one touches it), and nudge gently toward the
mouse/scroll position for a subtle sense of depth — nothing literal, no lightbulb, just
soft diagonal gradients of burgundy/pink light.

## Approach
- Render the rays as simple `<div>`s with a `linear-gradient` background (cheap,
  GPU-friendly — no need for canvas/WebGL for something this subtle).
- Use Svelte's `spring` store (from `svelte/motion`) to smoothly chase a target angle/
  offset — springs give natural, slightly bouncy easing for free.
- Idle animation: a slow `requestAnimationFrame` loop (or even simpler, a CSS
  `@keyframes` drift) so the rays sway gently on their own.
- Mouse/scroll influence: update the spring's target based on pointer position or
  scroll progress; the spring smooths out the motion so it never feels jittery.
- Respect `prefers-reduced-motion`: fall back to a static (or very minimal) version.

## Sketch

```svelte
<!-- src/lib/components/LightRays.svelte -->
<script>
  import { spring } from 'svelte/motion';
  import { onMount } from 'svelte';

  // one entry per founder — three rays, subtle nod to "three Luminaries"
  const rays = [
    { color: 'var(--color-burgundy)', baseAngle: -12, length: 140 },
    { color: 'var(--color-pink)', baseAngle: 0, length: 160 },
    { color: 'var(--color-burgundy)', baseAngle: 12, length: 140 },
  ];

  // pointer influence, smoothed with a spring so motion never feels jittery
  const pointer = spring({ x: 0.5, y: 0.5 }, { stiffness: 0.05, damping: 0.4 });

  let reduceMotion = false;

  onMount(() => {
    reduceMotion = window.matchMedia('(prefers-reduced-motion: reduce)').matches;
    if (reduceMotion) return; // skip listeners entirely, keep it static + cheap

    const handlePointerMove = (e) => {
      pointer.set({ x: e.clientX / window.innerWidth, y: e.clientY / window.innerHeight });
    };
    window.addEventListener('pointermove', handlePointerMove);
    return () => window.removeEventListener('pointermove', handlePointerMove);
  });
</script>

<div class="rays" aria-hidden="true">
  {#each rays as ray, i}
    <div
      class="ray"
      style="
        --angle: {ray.baseAngle + ($pointer.x - 0.5) * 8}deg;
        --length: {ray.length}%;
        --color: {ray.color};
        --delay: {i * 0.6}s;
      "
    ></div>
  {/each}
</div>

<style>
  .rays {
    position: absolute;
    inset: 0;
    overflow: hidden;
    pointer-events: none;
    z-index: 0;
  }

  .ray {
    position: absolute;
    top: -20%;
    left: 50%;
    width: var(--length);
    height: 160%;
    background: linear-gradient(
      to bottom,
      color-mix(in srgb, var(--color) 35%, transparent),
      transparent 70%
    );
    transform-origin: top center;
    transform: translateX(-50%) rotate(var(--angle));
    filter: blur(24px);
    animation: drift 12s ease-in-out infinite;
    animation-delay: var(--delay);
    transition: transform 0.6s ease-out;
  }

  @keyframes drift {
    0%, 100% { opacity: 0.6; }
    50% { opacity: 0.9; }
  }

  @media (prefers-reduced-motion: reduce) {
    .ray {
      animation: none;
      transition: none;
    }
  }
</style>
```

Usage in the hero:

```svelte
<!-- src/routes/+page.svelte -->
<section class="hero">
  <LightRays />
  <div class="hero-content">
    <h1>The Literacy Luminaries</h1>
    <p>Lighting the Way to Literacy</p>
    <!-- three pathway cards, intro copy, etc. -->
  </div>
</section>
```

## Why this is a good first "fun" feature
- Contained to one component — doesn't entangle with routing, CMS data, or the rest
  of the app, so it's low-risk to experiment with.
- Uses exactly the Svelte features that make the framework enjoyable (`spring`,
  reactive `$pointer` auto-subscription, scoped styles) without needing a heavy
  external animation library.
- Directly ties back to the brand brief: subtle, not literal; three-part; light/
  illumination themed; respects accessibility (reduced motion).
- Easy next iterations once it exists: tie ray angle to scroll position instead of
  (or in addition to) pointer position, add a very slow color shift, or reveal the
  three pathway cards with a `transition:fly` staggered by index right after this.

---

# Showcase Sites & Examples (Svelte/SvelteKit)

Curated toward things relevant to this project: marketing sites with subtle animation,
CMS-driven builds (especially Sanity/Storyblok pairings), and general "what's possible"
inspiration. Worth 20–30 minutes of browsing before you start building — good for
spotting motion/layout ideas to steal.

## Curated lists (best starting points)
- **Made with Svelte — Website category** — https://madewithsvelte.com/website —
  tagged directory of real Svelte/SvelteKit sites; this is the right category filter
  for marketing-site-style examples rather than apps/dev-tools. (Verified working.)
- **Awesome SvelteKit** — https://github.com/janosh/awesome-sveltekit (browsable at
  https://awesome-sveltekit.janosh.dev) — real production sites "in the wild" with
  their tech stack listed (many use Tailwind, Sanity, Storyblok, GSAP — the same
  pieces discussed in this plan).
- **Awwwards — Svelte tag** — https://www.awwwards.com/websites/svelte — award-winning,
  highly animated Svelte sites; good for visual/motion inspiration even where the
  content isn't relevant. **Note:** this one is known to 403 for some visitors (likely
  their bot/traffic protection reacting to something about the requester — ad blocker,
  network, VPN, etc.), even though it loads fine in other contexts. If it 403s for you,
  don't burn time troubleshooting it — just skip to the other lists below, which cover
  the same ground.
- **Sanity's framework showcase/exchange** — https://www.sanity.io/exchange —
  browse from here rather than trusting a deep-linked filter URL; I tried a direct
  `?framework=svelte` link and it didn't reliably filter when fetched directly, so
  it's safer to land on the exchange page and filter manually in-browser.

## Specific sites worth a look
- **Syntax.fm** (syntax.fm) — popular dev podcast site built with SvelteKit; clean,
  content-forward, good example of a simple, fast, professional marketing-adjacent
  site (not over-animated) — a good "restrained, professional" reference point, which
  is closer to this project's tone than a flashy agency site would be. (Verified
  working.)
- **Significa.co** — design/dev agency site, SvelteKit + Storyblok + Tailwind +
  Matter.js physics touches — good example of a CMS-driven marketing site with a bit
  of "fun" physics-based interactivity layered on top, similar spirit to the light-rays
  idea above but more elaborate. (Verified working.)
- **Spotify, The New York Times, IKEA, 1Password** (listed as Svelte users on
  svelte.dev) — good reminder that Svelte scales from tiny hobby sites to major
  companies; not all of these use SvelteKit for their whole marketing site, but
  reassuring as a "is this a serious enough choice" data point.

> Note: one previously-listed example (a "pixelmate.au" personal/agency site using
> SvelteKit + Sanity + Tailwind) turned out to be dead — the domain no longer
> resolves. Small showcase sites like that come and go; if a link in this doc ever
> 404s or times out, treat it as expected churn rather than something wrong with your
> setup, and lean on the curated lists above (which stay current) rather than one-off
> links.

## Learning-oriented resources (less "showcase," more "how do I build that")
- **joyofcode.xyz — "Create Amazing User Interfaces Using Animation With Svelte"**
  (https://joyofcode.xyz/animation-with-svelte) — walks through `tweened`/`spring`,
  transitions, and FLIP animations with practical examples; a good next read after
  this sketch.
- **Svelte's official interactive playground** (https://svelte.dev/playground) —
  has runnable examples for transitions, motion (tweened/spring), bindings, etc. —
  genuinely the fastest way to "try before you build," since you can edit and see
  results instantly with zero local setup.

## Suggested browsing order
1. Skim **Made with Svelte** "Website" category for 10 minutes — get a general feel.
2. Look at **Significa.co** and browse the **Sanity exchange** — the closest examples
   to the actual planned stack (SvelteKit + a headless CMS + Tailwind).
3. Play with the **transitions/motion examples in the Svelte Playground** — this will
   make the code sketch above click much faster than reading it cold.
4. Optionally browse **Awwwards' Svelte tag** purely for visual/motion inspiration,
   treating it as a mood board rather than a technical reference (a lot of those sites
   are far more elaborate than this project needs) — skip this one if it 403s for you,
   it's not essential.
