<script setup lang="ts">
interface Step {
  label: string;
  detail?: string;
}

interface Branch extends Step {
  tone: "positive" | "highlight";
}

withDefaults(
  defineProps<{
    steps?: Step[];
    branches?: Branch[];
  }>(),
  {
    steps: () => [
      { label: "Push", detail: "Change ships behind a flag" },
      { label: "Small % of traffic", detail: "Real users, limited blast radius" },
      { label: "Telemetry", detail: "Error budget & p99 latency, watched live" },
    ],
    // Auto-rollback is the point of the pattern, not a failure state — it
    // gets the highlight treatment, not the red reserved for negative numbers.
    branches: () => [
      { label: "Ship to 100%", detail: "Telemetry holds", tone: "positive" },
      { label: "Auto-rollback", detail: "Automatic — before anyone gets paged", tone: "highlight" },
    ],
  },
);
</script>

<template>
  <div class="wwt-progdelivery">
    <div class="wwt-progdelivery__flow">
      <template v-for="step in steps" :key="step.label">
        <div class="wwt-progdelivery__node">
          <div class="wwt-progdelivery__label">{{ step.label }}</div>
          <p v-if="step.detail" class="wwt-progdelivery__detail">{{ step.detail }}</p>
        </div>
        <div class="wwt-progdelivery__arrow" aria-hidden="true">&#8594;</div>
      </template>
    </div>

    <div class="wwt-progdelivery__branches">
      <div
        v-for="branch in branches"
        :key="branch.label"
        class="wwt-progdelivery__branch"
        :class="`wwt-progdelivery__branch--${branch.tone}`"
      >
        <div class="wwt-progdelivery__label">{{ branch.label }}</div>
        <p v-if="branch.detail" class="wwt-progdelivery__detail">{{ branch.detail }}</p>
      </div>
    </div>
  </div>
</template>

<style scoped>
.wwt-progdelivery {
  display: flex;
  align-items: center;
  gap: var(--wwt-space-6);
  /* default.vue's content area is a column flexbox; flex:1 (not height:100%)
     is what actually makes this grow to fill the space below the H1. */
  flex: 1;
  min-height: 0;
}

.wwt-progdelivery__flow {
  display: flex;
  align-items: center;
  gap: var(--wwt-space-4);
  flex: 2;
}

.wwt-progdelivery__node {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: var(--wwt-space-2);
  padding: var(--wwt-space-4) var(--wwt-space-6);
  border-radius: 10px;
  background: var(--wwt-primary-lightest);
}

.wwt-progdelivery__arrow {
  flex-shrink: 0;
  font-size: 24px;
  color: var(--wwt-ink-muted);
}

.wwt-progdelivery__label {
  font-size: var(--wwt-text-h2);
  font-weight: 600;
  color: var(--wwt-ink-base);
}

.wwt-progdelivery__detail {
  margin: 0;
  font-size: var(--wwt-text-caption);
  color: var(--wwt-ink-muted);
}

.wwt-progdelivery__branches {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: var(--wwt-space-4);
  padding-left: var(--wwt-space-6);
  border-left: 2px solid var(--wwt-ink-muted);
}

.wwt-progdelivery__branch {
  padding: var(--wwt-space-4);
  border-radius: 10px;
  display: flex;
  flex-direction: column;
  gap: var(--wwt-space-1);
}

.wwt-progdelivery__branch--positive {
  background: rgba(51, 158, 238, 0.1);
}

.wwt-progdelivery__branch--highlight {
  background: var(--wwt-secondary-base);
  color: var(--wwt-ink-white);
}

.wwt-progdelivery__branch--highlight .wwt-progdelivery__label,
.wwt-progdelivery__branch--highlight .wwt-progdelivery__detail {
  color: var(--wwt-ink-white);
}
</style>
