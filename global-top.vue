<!--
  Recurring "Don't Panic" motif badge, per the deck's visual style guide.
  Slidev auto-discovers `global-top.vue` at the project root and renders it
  once, above every slide (see https://sli.dev/custom/global-layers).

  This is intentionally self-contained rather than themed off `--wwt-*`
  tokens: it has to stay legible over the theme's own navy `cover` / `section`
  / `end` backgrounds, over a `quote` slide with `dark: true`, and over the
  full-bleed photo on the dog-on-a-bicycle slide — a fixed light chip with its
  own shadow reads reliably against all of those without needing to know which
  slide is currently showing.
-->
<template>
  <div class="dp-badge-layer" aria-hidden="true">
    <span class="dp-badge">Don't Panic</span>
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
  padding: 0.35em 0.85em;
  border-radius: 999px;
  background: rgba(255, 255, 255, 0.94);
  border: 1px solid #99cff7;
  box-shadow: 0 1px 3px rgba(10, 11, 25, 0.2);
  font-size: 12px;
  font-weight: 700;
  letter-spacing: 0.06em;
  color: #1c0087;
  white-space: nowrap;
}
</style>
