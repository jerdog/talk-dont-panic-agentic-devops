<script setup lang="ts">
import { computed } from "vue";

const props = withDefaults(
  defineProps<{
    /** Adds the three dip-phase annotations and the tuition-cost callout. */
    labeled?: boolean;
  }>(),
  { labeled: false },
);

// Curve anchor points, in a fixed 400-wide drawing area (height varies with
// `labeled`, since that variant reserves extra room at the bottom for
// annotations).
const start = { x: 20, y: 90 };
const trough = { x: 190, y: 180 };
const end = { x: 380, y: 30 };

const curvePath = `M${start.x},${start.y} C90,110 140,170 ${trough.x},${trough.y} C240,190 300,80 ${end.x},${end.y}`;

const VIEW_W = 400;
const viewH = computed(() => (props.labeled ? 270 : 220));
const viewBox = computed(() => `0 0 ${VIEW_W} ${viewH.value}`);
// The frame div is given this exact aspect-ratio via CSS, so the SVG never
// needs to letterbox inside it — which in turn means the HTML label overlay
// below can use simple x/width, y/height percentages and land exactly where
// the SVG coordinates put them, with no offset math.
const aspectRatio = computed(() => `${VIEW_W} / ${viewH.value}`);

// Evenly split across the curve's own width (20-380), not the tighter
// 20-130-245-300 bands used previously — those left "Pipeline adaptation"
// squeezed into a 55-unit band, so its label collided with its neighbor
// regardless of how the frame itself was sized.
const phases = [
  { x1: 20, x2: 140, label: "Learning curve" },
  { x1: 140, x2: 260, label: "Verification tax" },
  { x1: 260, x2: 380, label: "Pipeline adaptation" },
];

const phaseTickXs = [140, 260];

function leftPct(x: number) {
  return `${(x / VIEW_W) * 100}%`;
}
function topPct(y: number) {
  return `${(y / viewH.value) * 100}%`;
}
</script>

<template>
  <div class="wwt-jcurve">
    <div class="wwt-jcurve__frame" :style="{ aspectRatio }">
      <svg class="wwt-jcurve__svg" :viewBox="viewBox" preserveAspectRatio="none">
        <!-- reference line at the starting level, so the dip below it reads clearly -->
        <line
          :x1="start.x"
          :y1="start.y"
          :x2="end.x"
          :y2="start.y"
          class="wwt-jcurve__reference"
        />

        <template v-if="labeled">
          <line
            v-for="x in phaseTickXs"
            :key="x"
            :x1="x"
            y1="200"
            :x2="x"
            y2="215"
            class="wwt-jcurve__tick"
          />
        </template>

        <path :d="curvePath" class="wwt-jcurve__path" fill="none" />

        <circle :cx="start.x" :cy="start.y" r="5" class="wwt-jcurve__point" />
        <circle :cx="trough.x" :cy="trough.y" r="5" class="wwt-jcurve__point wwt-jcurve__point--trough" />
        <circle :cx="end.x" :cy="end.y" r="5" class="wwt-jcurve__point" />

        <line
          v-if="labeled"
          :x1="trough.x"
          :y1="trough.y"
          :x2="trough.x"
          y2="250"
          class="wwt-jcurve__leader"
        />
      </svg>

      <!-- Rendered as HTML, not SVG <text>: SVG text scales with the
           viewBox, so a "12px" label balloons once the SVG is stretched to
           fill the slide, which is what caused these to overlap. Real HTML
           text stays a real 12px regardless of how big the curve gets. -->
      <template v-if="labeled">
        <span
          v-for="(phase, i) in phases"
          :key="i"
          class="wwt-jcurve__phase-label"
          :style="{ left: leftPct((phase.x1 + phase.x2) / 2), top: topPct(222) }"
        >
          {{ phase.label }}
        </span>
        <span
          class="wwt-jcurve__callout"
          :style="{ left: leftPct(trough.x), top: topPct(256) }"
        >
          The tuition cost, not the failure.
        </span>
      </template>
    </div>
  </div>
</template>

<style scoped>
.wwt-jcurve {
  width: 100%;
  /* default.vue's content area is a column flexbox; flex:1 (not height:100%)
     is what actually makes this grow to fill the space below the H1. */
  flex: 1;
  min-height: 0;
  display: flex;
  align-items: center;
  justify-content: center;
}

.wwt-jcurve__frame {
  position: relative;
  /* height:100% (not width:100%/max-height) on purpose, mirroring how the
     original plain-SVG version sized itself: .wwt-jcurve's flex-allocated
     height is the one dimension that's reliably bounded here, so deriving
     width:auto from that via aspect-ratio reproduces the same well-
     proportioned, letterboxed sizing the old width:100%;height:100%; SVG +
     preserveAspectRatio="meet" got "for free" — just done in CSS instead of
     inside the SVG's viewBox math, which is what let the HTML label overlay
     below share its coordinate space exactly. */
  height: 100%;
  width: auto;
  max-width: 100%;
}

.wwt-jcurve__svg {
  display: block;
  width: 100%;
  height: 100%;
}

.wwt-jcurve__path {
  stroke: var(--wwt-primary-base);
  stroke-width: 4;
  stroke-linecap: round;
}

.wwt-jcurve__reference {
  stroke: var(--wwt-ink-muted);
  stroke-width: 1.5;
  stroke-dasharray: 6 6;
  opacity: 0.5;
}

.wwt-jcurve__point {
  fill: var(--wwt-bg-base);
  stroke: var(--wwt-primary-base);
  stroke-width: 3;
}

.wwt-jcurve__point--trough {
  fill: var(--wwt-secondary-base);
  stroke: var(--wwt-secondary-base);
}

.wwt-jcurve__tick {
  stroke: var(--wwt-ink-muted);
  stroke-width: 1.5;
  opacity: 0.5;
}

.wwt-jcurve__phase-label,
.wwt-jcurve__callout {
  position: absolute;
  transform: translateX(-50%);
  text-align: center;
  white-space: nowrap;
}

.wwt-jcurve__phase-label {
  font-size: 13px;
  font-weight: 600;
  color: var(--wwt-ink-muted);
}

.wwt-jcurve__callout {
  font-size: var(--wwt-text-body);
  font-weight: 600;
  color: var(--wwt-secondary-base);
}
</style>
