<script setup lang="ts">
type Tone = "negative" | "warning" | "mid" | "positive" | "strong";

interface Cluster {
  id: string;
  pct: number;
  name: string;
  tone: Tone;
}

// DORA 2025's seven team archetypes, n=5,000 — C4 and C5 combined into one
// cell per the outline (both "solid work at a moderate pace," 7% + 15%).
// Cell width is proportional to share of teams, so the mosaic itself shows
// the 38%-in-difficulty / 40%-high-performing split before the speaker says it.
withDefaults(
  defineProps<{
    clusters?: Cluster[];
    source?: string;
  }>(),
  {
    source: undefined,
    clusters: () => [
      { id: "C1", pct: 10, name: "Foundational challenges", tone: "negative" },
      { id: "C2", pct: 11, name: "The legacy bottleneck", tone: "warning" },
      { id: "C3", pct: 17, name: "Constrained by process", tone: "warning" },
      { id: "C4+C5", pct: 22, name: "High-impact & stable teams", tone: "mid" },
      { id: "C6", pct: 20, name: "Pragmatic performers", tone: "positive" },
      { id: "C7", pct: 20, name: "Harmonious high-achievers", tone: "strong" },
    ],
  },
);
</script>

<template>
  <div class="wwt-archetypes-wrap">
    <div class="wwt-archetypes">
      <div
        v-for="cluster in clusters"
        :key="cluster.id"
        class="wwt-archetypes__cell"
        :class="`wwt-archetypes__cell--${cluster.tone}`"
        :style="{ flexGrow: cluster.pct }"
      >
        <span class="wwt-archetypes__id">{{ cluster.id }}</span>
        <span class="wwt-archetypes__pct">{{ cluster.pct }}%</span>
        <span class="wwt-archetypes__name">{{ cluster.name }}</span>
      </div>
    </div>
    <p v-if="source" class="wwt-archetypes__source">{{ source }}</p>
  </div>
</template>

<style scoped>
.wwt-archetypes-wrap {
  display: flex;
  flex-direction: column;
  gap: var(--wwt-space-3);
  /* default.vue's content area is a column flexbox; flex:1 (not height:100%)
     is what actually makes this grow to fill the space below the H1. */
  flex: 1;
  min-height: 0;
}

.wwt-archetypes {
  display: flex;
  align-items: stretch;
  gap: var(--wwt-space-2);
  flex: 1;
  min-height: 0;
}

.wwt-archetypes__source {
  margin: 0;
  font-size: var(--wwt-text-caption);
  color: var(--wwt-ink-muted);
}

.wwt-archetypes__cell {
  flex-basis: 0;
  min-width: 0;
  overflow: hidden;
  display: flex;
  flex-direction: column;
  gap: var(--wwt-space-2);
  padding: var(--wwt-space-6) var(--wwt-space-2);
  border-radius: 8px;
  border-top: 5px solid;
}

.wwt-archetypes__id {
  font-size: var(--wwt-text-caption);
  font-weight: 700;
  letter-spacing: 0.04em;
  color: var(--wwt-ink-muted);
}

.wwt-archetypes__pct {
  font-size: 40px;
  font-weight: 300;
  line-height: 1;
}

.wwt-archetypes__name {
  /* Slightly under --wwt-text-caption: C1 is the narrowest column (10% of
     the row) and "Foundational" needs the extra room to hyphenate cleanly
     instead of falling back to a mid-word break. */
  font-size: 14px;
  font-weight: 600;
  color: var(--wwt-ink-base);
  /* Narrow cells (C1 is ~10% of the row) can't always break a long word on
     whitespace alone. hyphens:auto breaks at syllables with a hyphen glyph;
     break-word is only the fallback so nothing ever bleeds into the next
     cell if the browser can't find a hyphenation point. */
  hyphens: auto;
  -webkit-hyphens: auto;
  overflow-wrap: break-word;
}

.wwt-archetypes__cell--negative {
  background: rgba(238, 40, 42, 0.08);
  border-top-color: var(--wwt-accent6-base);
}
.wwt-archetypes__cell--negative .wwt-archetypes__pct {
  color: var(--wwt-accent6-base);
}

.wwt-archetypes__cell--warning {
  background: rgba(251, 85, 14, 0.08);
  border-top-color: var(--wwt-accent5-base);
}
.wwt-archetypes__cell--warning .wwt-archetypes__pct {
  color: var(--wwt-accent5-base);
}

.wwt-archetypes__cell--mid {
  /* Diluted to match the --positive/--strong pattern below — the solid
     --wwt-primary-lightest this used before was nearly the same pale blue
     as --wwt-primary-light text, so the "22%" was almost unreadable. */
  background: rgba(102, 182, 242, 0.12);
  border-top-color: var(--wwt-primary-light);
}
.wwt-archetypes__cell--mid .wwt-archetypes__pct {
  color: var(--wwt-secondary-base);
}

.wwt-archetypes__cell--positive {
  background: rgba(51, 158, 238, 0.12);
  border-top-color: var(--wwt-primary-medium);
}
.wwt-archetypes__cell--positive .wwt-archetypes__pct {
  color: var(--wwt-primary-medium);
}

.wwt-archetypes__cell--strong {
  background: rgba(0, 134, 234, 0.12);
  border-top-color: var(--wwt-primary-base);
}
.wwt-archetypes__cell--strong .wwt-archetypes__pct {
  color: var(--wwt-primary-base);
}
</style>
