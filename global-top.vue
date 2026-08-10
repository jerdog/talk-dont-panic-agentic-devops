<!--
  Recurring "Don't Panic" motif badge, per the deck's visual style guide.
  Slidev auto-discovers `global-top.vue` at the project root and renders it
  once, above every slide (see https://sli.dev/custom/global-layers).

  Uses the real Hitchhiker's Guide cover art (public/images/dont-panic.png,
  678x452, its own opaque black background) rather than a themed text chip —
  the photo's black backdrop already reads reliably over the theme's navy
  `cover` / `section` / `end` backgrounds, a `quote` slide with `dark: true`,
  and the full-bleed photo on the dog-on-a-bicycle slide, without needing to
  know which slide is currently showing.

  Suppressed on `layout: cover` — S1 already carries its own big "Don't Panic"
  title treatment plus the WWT logo in that same top-right corner; the badge
  there reads as clutter rather than a callback.

  To suppress it on any other slide, set `hideBadge: true` in that slide's
  own frontmatter:

    ---
    layout: image-full
    hideBadge: true
    ---
-->
<script setup lang="ts">
import { useNav } from "@slidev/client";
const { currentLayout, currentFrontmatter } = useNav();
</script>

<template>
  <div
    v-if="currentLayout !== 'cover' && !currentFrontmatter?.hideBadge"
    class="dp-badge-layer"
    aria-hidden="true"
  >
    <img class="dp-badge" src="/images/dont-panic.png" alt="" />
  </div>
</template>

<style scoped>
.dp-badge-layer {
  position: absolute;
  inset: 0;
  pointer-events: none;
  z-index: 20;
}

.dp-badge {
  position: absolute;
  top: 1.5rem;
  right: 2rem;
  height: 32px;
  width: auto;
  /* No aspect-ratio/object-fit: those require hardcoding the source image's
     pixel dimensions, and go stale (silently cropping the image) the moment
     that file gets swapped for one with a different ratio — which is
     exactly what happened when dont-panic.jpeg (678x452) became
     dont-panic.png (510x285). Natural width at a fixed height needs neither. */
  border-radius: 6px;
  transform: rotate(-8deg);
  box-shadow:
    0 6px 10px rgba(10, 11, 25, 0.35),
    0 2px 4px rgba(10, 11, 25, 0.25);
}
</style>

<style>
/* Unscoped on purpose: global-top.vue mounts once, above every slide, so
   this reaches into every theme layout's markup — a scoped block can't,
   since Vue only stamps its hash on this component's own root.
   Every content layout (default/stats/timeline/agenda/comparison/process/
   two-cols/image-feature/demo/team/quote) renders a top-left WWT monogram
   duplicating the one Footer.vue already shows bottom-left. Hide the
   layout copies; keep the footer's. */
.wwt-monogram-mark {
  display: none;
}
.wwt-footer__monogram {
  display: block;
}

/* WCAG 2.2 contrast fix (theme bug, patched here since layouts/*.vue lives
   in node_modules): timeline.vue's date label uses --wwt-secondary-base as
   text directly on --wwt-bg-base. That token doesn't flip for dark mode —
   measured 1.30:1 against the 4.5:1 minimum for normal text, i.e. the date
   on S36 (Dec 2025, Mar 2 2026, etc.) goes all but invisible.
   --wwt-secondary-lightest clears it (7.48:1).

   !important is load-bearing, not defensive styling: `.dark .foo` and the
   component's own scoped `.foo[data-v-hash]` are equal CSS specificity
   (two class-level selectors each), so without it the winner depends on
   stylesheet load order — verified here to go the wrong way, silently, with
   no error. Same pattern the deck's own slides.md already uses for
   :deep() font-size overrides. */
.dark .wwt-timeline__date {
  color: var(--wwt-secondary-lightest) !important;
}

/* Same --wwt-secondary-base dark-mode gap, this time in components/ own
   JCurve.vue and McpSchematic.vue (S10, S45). These live in this repo, not
   node_modules, so the natural fix is a :global(.dark) rule inside each
   component's own scoped style block — but that syntax silently fails to
   compile in this project's Vite/Vue toolchain (verified: the rule never
   reaches the served stylesheet, no build error, no console warning).
   Parked here instead, next to the timeline fix it mirrors — same
   !important reasoning applies. */
.dark .wwt-jcurve__point--trough {
  fill: var(--wwt-secondary-lightest) !important;
  stroke: var(--wwt-secondary-lightest) !important;
}
.dark .wwt-jcurve__callout {
  color: var(--wwt-secondary-lightest) !important;
}
.dark .wwt-mcp__chip {
  color: var(--wwt-secondary-lightest) !important;
}
.dark .wwt-mcp__tag {
  color: var(--wwt-secondary-lightest) !important;
}

/* Same --wwt-secondary-base dark-mode gap, but as a GRAPHICAL fill rather
   than text: BarChart.vue's "secondary" bar tone (the "Feature branch"
   bars/legend swatch on S17 — the deck's own MUST-NAIL chart per its
   speaker notes) measured 1.30:1 in dark mode against the 3:1 minimum for
   non-text graphical objects (1.4.11) — the bars were nearly invisible
   against the dark background. --wwt-secondary-lightest clears it easily. */
.dark .wwt-barchart__bar--secondary {
  background: var(--wwt-secondary-lightest) !important;
}
.dark .wwt-barchart__swatch--secondary {
  background: var(--wwt-secondary-lightest) !important;
}

/* Same --wwt-secondary-base dark-mode gap, this time in slides.md's own
   per-slide style block (S12, "Three reports, three conclusions" —
   Optimistic/Neutral/Pessimistic tags). Confirmed the same :global(.dark)
   compile failure applies to Slidev's per-slide styles too, not just
   component SFCs — verified via computed style, not just visually; an
   earlier pass here mistakenly signed this fix off from a screenshot alone.
   --wwt-primary-light clears it (7.02:1 on this card's dark-mode wash). */
.dark .wwt-threeviews__tag {
  color: var(--wwt-primary-light) !important;
}

/* Deck-wide quote-layout typography, requested 2026-08-07: bigger default
   quote text, smaller italic attribution. Applies in both light and dark
   quote slides (quote.vue's dark:true variant only swaps background/ink
   color, not size), so this isn't a `.dark`-gated rule like the fixes above.

   quote.vue's own base rules aren't !important, so this only needs
   !important to jump into that higher-priority bucket — once there, it
   reliably beats quote.vue's plain scoped rule regardless of load order,
   the same way the `.dark` fixes above do. It also reliably LOSES to the
   three slides that hand-tune .wwt-quote__text per slide (S15/S36/S51,
   via :deep(...) !important in slides.md): their compiled selector carries
   an extra [data-v-hash] attribute, which out-specifies this plain class
   selector even within the !important bucket — so their sizes keep
   overriding this default, by construction, no load-order gamble needed. */
.wwt-quote__text {
  font-size: 44px !important;
}
.wwt-quote__attribution {
  font-size: 20px !important;
  font-style: italic !important;
}
</style>
