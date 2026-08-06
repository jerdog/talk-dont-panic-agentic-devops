<script setup lang="ts">
withDefaults(
  defineProps<{
    providers?: string[];
    tags?: string[];
  }>(),
  {
    providers: () => ["AWS", "GCP", "Azure"],
    tags: () => ["Zero-trust", "Portable", "Auditable"],
  },
);
</script>

<template>
  <div class="wwt-mcp">
    <div class="wwt-mcp__providers">
      <span v-for="p in providers" :key="p" class="wwt-mcp__chip">{{ p }}</span>
    </div>

    <svg class="wwt-mcp__lines" viewBox="0 0 300 60" preserveAspectRatio="none" aria-hidden="true">
      <line x1="50" y1="0" x2="150" y2="60" />
      <line x1="150" y1="0" x2="150" y2="60" />
      <line x1="250" y1="0" x2="150" y2="60" />
    </svg>

    <div class="wwt-mcp__hub">
      <span class="wwt-mcp__hub-title">MCP</span>
      <span class="wwt-mcp__hub-detail">Portable reasoning engine</span>
    </div>

    <svg class="wwt-mcp__lines" viewBox="0 0 300 40" preserveAspectRatio="none" aria-hidden="true">
      <line x1="150" y1="0" x2="150" y2="40" />
    </svg>

    <div class="wwt-mcp__tags">
      <span v-for="t in tags" :key="t" class="wwt-mcp__tag">{{ t }}</span>
    </div>
  </div>
</template>

<style scoped>
.wwt-mcp {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  /* default.vue's content area is a column flexbox; flex:1 (not height:100%)
     is what actually makes this grow to fill the space below the H1. */
  flex: 1;
  min-height: 0;
}

.wwt-mcp__providers {
  display: flex;
  justify-content: space-around;
  width: 100%;
}

.wwt-mcp__chip {
  padding: var(--wwt-space-2) var(--wwt-space-6);
  border-radius: 999px;
  border: 1px solid var(--wwt-primary-light);
  color: var(--wwt-secondary-base);
  font-weight: 600;
  font-size: var(--wwt-text-body);
  /* Dark-mode override lives in global-top.vue's unscoped style block:
     :global(.dark) inside a component's scoped style silently fails to
     compile in this project's Vite/Vue toolchain (confirmed — the rule
     never reaches the served stylesheet, no build error). --wwt-secondary-base
     doesn't flip for dark mode, measured 1.30:1 against the 4.5:1 minimum
     for normal text. */
}

.wwt-mcp__lines {
  width: 100%;
  height: 3.5rem;
  overflow: visible;
}

.wwt-mcp__lines line {
  stroke: var(--wwt-primary-light);
  stroke-width: 2;
}

.wwt-mcp__hub {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: var(--wwt-space-1);
  padding: var(--wwt-space-6) var(--wwt-space-8);
  border-radius: 16px;
  background: var(--wwt-secondary-base);
  color: var(--wwt-ink-white);
}

.wwt-mcp__hub-title {
  font-size: var(--wwt-text-h1);
  font-weight: 700;
}

.wwt-mcp__hub-detail {
  font-size: var(--wwt-text-caption);
  opacity: 0.85;
}

.wwt-mcp__tags {
  display: flex;
  gap: var(--wwt-space-4);
}

.wwt-mcp__tag {
  padding: var(--wwt-space-2) var(--wwt-space-4);
  border-radius: 999px;
  background: var(--wwt-primary-lightest);
  color: var(--wwt-secondary-base);
  font-size: var(--wwt-text-caption);
  font-weight: 600;
  /* Dark-mode override lives in global-top.vue — see the note on
     .wwt-mcp__chip above for why it can't live here. Measured against the
     dark-mode composite of --wwt-primary-lightest specifically: 1.03:1 vs
     the 4.5:1 minimum. */
}
</style>
