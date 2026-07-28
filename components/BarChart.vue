<script setup lang="ts">
import { computed } from "vue";

type Tone = "primary" | "secondary" | "negative" | "flat";

interface Bar {
  value: number;
  tone?: Tone;
  /** Overrides the printed value, e.g. "flat" for a value near zero. */
  display?: string;
}

interface BarGroup {
  label: string;
  bars: Bar[];
}

interface LegendItem {
  label: string;
  tone: Tone;
}

const props = withDefaults(
  defineProps<{
    groups: BarGroup[];
    legend?: LegendItem[];
    unit?: string;
    orientation?: "vertical" | "horizontal";
    source?: string;
    /** Reveal groups one at a time on click. */
    clicks?: boolean;
  }>(),
  {
    legend: undefined,
    unit: "",
    orientation: "vertical",
    source: undefined,
    clicks: false,
  },
);

const allValues = computed(() => props.groups.flatMap((g) => g.bars.map((b) => b.value)));
const maxPos = computed(() => Math.max(0, ...allValues.value));
const maxNeg = computed(() => Math.min(0, ...allValues.value));
const range = computed(() => maxPos.value - maxNeg.value || 1);
// % of the track reserved for the negative side of the baseline (0 when there are no negative values)
const negRegion = computed(() => (-maxNeg.value / range.value) * 100);
const posRegion = computed(() => 100 - negRegion.value);
const hasNegative = computed(() => maxNeg.value < 0);
// Exposed as a ready-made CSS length so the scoped <style> block can
// v-bind() it without embedding a template literal inside the binding.
const negRegionCss = computed(() => `${negRegion.value}%`);

function barRect(value: number) {
  if (value >= 0) {
    const length = maxPos.value ? (value / maxPos.value) * posRegion.value : 0;
    return props.orientation === "vertical"
      ? { bottom: `${negRegion.value}%`, height: `${length}%` }
      : { left: `${negRegion.value}%`, width: `${length}%` };
  }
  const length = maxNeg.value ? (value / maxNeg.value) * negRegion.value : 0;
  return props.orientation === "vertical"
    ? { top: `${posRegion.value}%`, height: `${length}%` }
    : { right: `${posRegion.value}%`, width: `${length}%` };
}

function display(bar: Bar) {
  if (bar.display) return bar.display;
  const sign = bar.value > 0 ? "+" : "";
  return `${sign}${bar.value}${props.unit}`;
}
</script>

<template>
  <div class="wwt-barchart" :class="`wwt-barchart--${orientation}`">
    <ul v-if="legend?.length" class="wwt-barchart__legend">
      <li v-for="(item, i) in legend" :key="i">
        <span class="wwt-barchart__swatch" :class="`wwt-barchart__swatch--${item.tone}`" />
        {{ item.label }}
      </li>
    </ul>

    <div class="wwt-barchart__body">
      <div
        v-for="(group, gi) in groups"
        :key="group.label"
        class="wwt-barchart__group"
        v-click="clicks ? gi + 1 : false"
      >
        <div class="wwt-barchart__track">
          <div v-if="hasNegative" class="wwt-barchart__zero" />
          <div v-for="(bar, bi) in group.bars" :key="bi" class="wwt-barchart__slot">
            <div
              class="wwt-barchart__bar"
              :class="`wwt-barchart__bar--${bar.tone ?? 'primary'}`"
              :style="barRect(bar.value)"
            >
              <span class="wwt-barchart__value">{{ display(bar) }}</span>
            </div>
          </div>
        </div>
        <div class="wwt-barchart__label">{{ group.label }}</div>
      </div>
    </div>

    <p v-if="source" class="wwt-barchart__source">{{ source }}</p>
  </div>
</template>

<style scoped>
.wwt-barchart {
  display: flex;
  flex-direction: column;
  gap: var(--wwt-space-4);
  /* default.vue's content area is a column flexbox; flex:1 (not height:100%)
     is what actually makes this grow to fill the space below the H1 — and
     it's load-bearing here, since the bars below are absolutely positioned
     by percentage and need a real, non-zero track height to measure against. */
  flex: 1;
  min-height: 0;
}

.wwt-barchart__legend {
  display: flex;
  gap: var(--wwt-space-6);
  list-style: none;
  padding: 0;
  margin: 0;
  font-size: var(--wwt-text-caption);
  color: var(--wwt-ink-muted);
}

.wwt-barchart__legend li {
  display: flex;
  align-items: center;
  gap: var(--wwt-space-2);
}

.wwt-barchart__swatch {
  width: 10px;
  height: 10px;
  border-radius: 2px;
  display: inline-block;
}

.wwt-barchart__swatch--primary {
  background: var(--wwt-primary-base);
}
.wwt-barchart__swatch--secondary {
  background: var(--wwt-secondary-base);
}
.wwt-barchart__swatch--negative {
  background: var(--wwt-accent6-base);
}
.wwt-barchart__swatch--flat {
  background: var(--wwt-ink-muted);
}

.wwt-barchart__body {
  flex: 1;
  display: flex;
  gap: var(--wwt-space-8);
  min-height: 0;
}

.wwt-barchart--horizontal .wwt-barchart__body {
  flex-direction: column;
  gap: var(--wwt-space-3);
}

.wwt-barchart__group {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: var(--wwt-space-2);
  min-width: 0;
}

.wwt-barchart--horizontal .wwt-barchart__group {
  flex-direction: row;
  align-items: center;
  gap: var(--wwt-space-4);
}

/* The track holds the shared zero baseline and one flex "slot" per bar,
   so grouped bars sit side by side while each is still positioned
   independently off the same baseline. */
.wwt-barchart__track {
  position: relative;
  flex: 1;
  display: flex;
  gap: var(--wwt-space-2);
  min-height: 0;
}

.wwt-barchart--horizontal .wwt-barchart__track {
  flex-direction: column;
}

.wwt-barchart__slot {
  position: relative;
  flex: 1;
}

.wwt-barchart__zero {
  position: absolute;
  z-index: 1;
  opacity: 0.4;
}

.wwt-barchart--vertical .wwt-barchart__zero {
  left: 0;
  right: 0;
  bottom: v-bind(negRegionCss);
  border-top: 1px dashed var(--wwt-ink-muted);
}

.wwt-barchart--horizontal .wwt-barchart__zero {
  top: 0;
  bottom: 0;
  left: v-bind(negRegionCss);
  border-left: 1px dashed var(--wwt-ink-muted);
}

.wwt-barchart__bar {
  position: absolute;
  border-radius: 4px;
  transition: all 0.4s ease;
}

.wwt-barchart--vertical .wwt-barchart__bar {
  left: 0;
  right: 0;
  min-height: 3px;
}

.wwt-barchart--horizontal .wwt-barchart__bar {
  top: 0;
  bottom: 0;
  min-width: 3px;
}

.wwt-barchart__bar--primary {
  background: var(--wwt-primary-base);
}
.wwt-barchart__bar--secondary {
  background: var(--wwt-secondary-base);
}
.wwt-barchart__bar--negative {
  background: var(--wwt-accent6-base);
}
.wwt-barchart__bar--flat {
  background: var(--wwt-ink-muted);
  opacity: 0.6;
}

.wwt-barchart__value {
  position: absolute;
  font-size: var(--wwt-text-caption);
  font-weight: 600;
  color: var(--wwt-ink-base);
  white-space: nowrap;
}

.wwt-barchart--vertical .wwt-barchart__value {
  bottom: 100%;
  left: 50%;
  transform: translateX(-50%);
  margin-bottom: 4px;
}

.wwt-barchart--horizontal .wwt-barchart__value {
  left: 100%;
  top: 50%;
  transform: translateY(-50%);
  margin-left: 8px;
}

.wwt-barchart__label {
  text-align: center;
  font-size: var(--wwt-text-caption);
  font-weight: 600;
  color: var(--wwt-ink-muted);
}

.wwt-barchart--horizontal .wwt-barchart__label {
  text-align: left;
  width: 11rem;
  flex-shrink: 0;
}

.wwt-barchart__source {
  margin: 0;
  font-size: var(--wwt-text-caption);
  color: var(--wwt-ink-muted);
}
</style>
