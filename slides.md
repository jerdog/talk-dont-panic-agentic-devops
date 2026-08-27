---
theme: wwt
title: "Don't Panic"
subtitle: A Hitchhiker's Guide to Agentic DevOps
info: |
  ## Don't Panic - A Hitchhiker's Guide to Agentic DevOps

  We've all seen the demos where an AI writes a "To-Do" app in thirty seconds.
  It's impressive, sure, in the same way a dog riding a bicycle is impressive -
  but it's not exactly how we build production-grade software. In the real
  world the SDLC is messy, our pipelines are held together by legacy YAML, and
  "moving fast" usually means "breaking things we didn't know existed."

  This talk looks past the hype and into the data: DORA, CircleCI, Sonar, and
  METR's own randomized controlled trial, plus what happened when Meta, Uber,
  and Amazon took agentic AI further than almost anyone else this year. AI is
  an amplifier - it makes strong teams stronger and struggling teams worse,
  faster. The question isn't what AI can build. It's how you ship this without
  making everyone furious.

  At the end, we'll have a roadmap: what to measure, what to budget for, and
  why the most important part of AI-driven DevOps isn't the code it generates,
  but the bottlenecks it actually clears.
presenterName: Jeremy Meiss
presenterRole:
layout: cover
duration: 45min
transition: slide-left
colorSchema: dark
---

<!--
A show of hands: who recognizes where the title of this talk is from? For those who don't, it's what's printed on the cover of the Hitchhiker's Guide to the Galaxy from the Douglas Adams book of the same name. He describes the book as the most important piece of advice ever offered to a being trying to navigate a chaotic universe. If that doesn't describe the current state of Software Development, much less our world today, I don't know what else does.
-->

---
# S2 - "DON'T PANIC" bookend (opens the talk)
layout: image
image: /images/dont-panic.png
backgroundSize: fill
hideBadge: true
title: "DON'T PANIC"
---

<!--
But before I tell you not to panic, I want to acknowledge that you probably already are. Maybe quietly. And you're not alone, because it shows up in standups disguised as "how do we feel about Q3 velocity?", or in Slack when someone asks "is anyone else's Copilot doing this?" It shows up in the CTO's office as "what's our AI strategy," which is usually just "are we falling behind" wearing a nicer shirt.
-->

---
# S3 - dog on a bicycle (needs a real photo - see public/images/dog-on-a-bicycle.svg)
layout: image
image: /images/golden-retriever-on-bicycle.png
backgroundSize: fit
imageAlt: A dog riding a bicycle.
---

<!--
Most of us are somewhere on that spectrum because we've all watched the same demos of some to-do app built in thirty seconds, or the SaaS product from an idea hatched over the weekend, or the LinkedIn post that opens with "I'm just an idea guy but...." Those demos or ideas are sometimes impressive, in roughly the way a dog riding a bicycle is impressive - the fact that it happens at all is remarkable, and also, you would not want to build production infrastructure the way that dog builds momentum.
-->

---
# S4 - "DON'T PANIC" slide again)
layout: image
image: /images/dont-panic.png
backgroundSize: fill
hideBadge: true
title: "DON'T PANIC"
---

<!--
So that's not what we are going to do today. No demo. No claim that agents will replace your team next quarter. Instead we want to give you a map, because for the last few years (and if we're honest, for longer than that) the industry has been asking the wrong question. We keep asking what AI can build. The question that actually matters, though, is the one your platform team will ask you in six weeks whether you're ready or not: how do you ship this without making everyone furious?

That's the talk.
-->

---
# About the speakers slide
layout: speaker
speakers:
  - name: Jeremy Meiss
    role: Tech Solution Architect
    company: WWT
    photo: /images/jeremy-meiss.png
    orgs:
      - name: DevOpsDays KC
      - name: CommunityDays KC
      - name: CDF Ambassador 2025-26
  # - name: PJ Hagerty
  #   role: Tech Solution Architect
  #   company: WWT
  #   photo: /images/pj-hagerty.png
---

# Jeremy Meiss

---
# S5 - anchor: AI is an amplifier
layout: quote
attribution: DevOps Research and Assessment (DORA), 2025
role: n = 5,000
---

AI is an amplifier.

<!--
DORA is a group that's been studying engineering performance for over a decade - they surveyed 5,000 technology professionals last year and ran more than a hundred hours of qualitative interviews. Their conclusion boiled down to one sentence worth writing on your hand: AI is an amplifier. It doesn't fix anything. It takes whatever's already true about your organization and makes more of it. High-performing teams get faster. Struggling teams get worse, faster. DORA also calls it a mirror, which I think is the same idea from a different angle - it shows you who you already were.
-->

---
# S6 - CircleCI throughput stat
layout: stats
title: "CI throughput increase is deceptive"
stats:
  - value: "+59%"
    label: CI throughput, YoY
    caption: 28M workflows analyzed - CircleCI 2026
  - value: "Top 5%"
    label: where almost all of it sits
    caption: median team +4% · bottom quartile +0%
---

<!--
Here's an example of what this looks like in practice. [click]CircleCI looked at 28 million CI workflows over the past year and found throughput was up 59% year over year. That's a significantly large number. [click]But almost all of that gain was from the top 5% of teams, who nearly doubled their output. The median team improved 4%. The bottom quarter improved not at all.

Same tools. Same models. Wildly different outcomes. That's the amplifier effect, in real operational data, not a survey.
-->

---
# S7 - Let's not panic
layout: two-cols-header
layoutClass: wwt-header-center wwt-cols-center
---

# Our three takeaways

::left::

![dont-panic](/images/dont-panic.png)

::right::

1. Why some teams are pulling ahead while most aren't
2. What the actual bottleneck is now - it has moved, and it has a name
3. A set of steps you can bring to your VP on Monday that fit in a Jira ticket rather than a strategy deck

<style>
  ol, ul {
    list-style-type: decimal;
  }
</style>

<!--
Three things I want you to leave with today: why some teams are pulling ahead while most aren't, what the actual bottleneck is now - it has moved, and it has a name - and a set of steps you can bring to your VP on Monday that fit in a Jira ticket rather than a strategy deck.

Don't panic. Let's get into it.
-->

---
# S7 - Section 1 opener ("Section 1: The Map of Where We Actually Are")
# number: dropped - with the 4 mid-talk dividers (S12/S24/S31/S39) hidden,
# a lone "01" numeral would read as a broken 1-of-6 sequence. Titled break
# instead; section.vue already guards this with v-if="$frontmatter?.number".
# Jeremy
layout: section
title: '"Mostly Harmless"'
---

<!--
"Mostly harmless" is what the Hitchhiker's Guide eventually settles on as its entire entry for planet Earth, after Ford Prefect submits fifteen minutes of dictated research that gets trimmed for space. It's also roughly what most CTOs will tell you about their AI strategy if you catch them off guard. Both are technically true and both leave out a lot.

So let's fill in the gaps. Before we get to any data, we need a shared sense of where teams actually sit, because "agentic DevOps" gets used loosely enough that it's stopped meaning much. Broadly, there are three places.
-->

---
# S8 - the three maturity tiers
layout: process
title: Where teams actually sit
steps:
  - title: AI-Assisted
    detail: Copilot, Cursor, ChatGPT for the odd question. 72% use it daily (Sonar 2026). Four tools per team, no governance.
  - title: AI-Optimized
    detail: An actual decision got made - strategy, tool selection, security review. Roughly the top quarter.
  - title: Agent-Augmented
    detail: Agents doing real end-to-end work. The human moves from in the loop to over the loop. The top 5%.
---

<!--
[click]Most of you are in the first one: AI-assisted. Developers using these tools individually, mostly for autocomplete and for explaining code they didn't write. There's rarely governance and rarely measurement. Sonar found that 72% of developers who've tried AI use it daily now, and the average team has four different tools running at once with nobody coordinating them. This isn't a bad place to start. It's just not a place to stay.

[click]Fewer of you are in the second: AI-optimized. There's a strategy, tool selection, security review, procurement that doesn't take six months. Task-level efficiency gets measured. This is roughly the top quarter of organizations.

[click]And a small number of you are in the third: agent-augmented. Agents doing real end-to-end work - opening pull requests, running test suites, proposing rollbacks at three in the morning instead of paging someone. The human moves from being in the loop on every step to being over the loop, setting policy and spot-checking outcomes. This is the top 5%, and it's the destination.
-->

---
# S9 - audience gut-check
layout: default
---

<div class="wwt-gutcheck">✋</div>

<style>
.wwt-gutcheck {
  flex: 1;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 200px;
}
</style>

<!--
Quick show of hands, if you're willing. How many of you would call your org agent-augmented? [pause] Now - how many would call it AI-assisted with a slide deck that claims agent-augmented? [pause] That gap is basically what this whole talk is about.
-->

---
# S10 - J-curve, unlabeled
layout: default
---

# It isn't a straight line

<JCurve />

<!--
Here's the part I think is most useful, and it comes from DORA's 2026 ROI report: the path between those three levels isn't a straight line up. It's a J-curve.
-->

---
# S11 - J-curve, labeled
layout: default
---

# The dip has a name

<JCurve labeled />

<!--
Serious AI adoption produces a dip before it produces growth, and three things cause it. A learning curve, since your team has to figure out new workflows. What DORA calls the verification tax, meaning the time spent checking whether AI output can actually be trusted. And pipeline adaptation, since your downstream systems - testing, review, deployment - have to scale to handle more volume than they were built for.

The trap, and DORA is explicit about this, is that leadership sees the dip and assumes the initiative is failing, so they pull funding right before the payoff would have shown up. The dip isn't the failure. Paying for it is the cost of getting anywhere. Pulling out early is the actual mistake.

Everything else in this talk lives somewhere inside that dip. So let's look at what it actually looks like.
-->

---
# S12 - Section 2 opener ("Section 2: Where the Bottleneck Moved").
# Hidden (v8 merge, 2026-07-31) - mid-talk section dividers cut for pacing.
# Its speaker-note opener (the Vogons/hyperspace-bypass beat) moved to the
# top of S13's notes below so the Hitchhiker callback survives the hide.
#
# PJ
layout: section
number: "02"
title: The Hyperspace Bypass
hide: true
---

<!--
Early in the Hitchhiker's Guide, Earth gets demolished to make way for a hyperspace bypass, and when people object, the Vogons point out that the plans have been on display in a planning office on Alpha Centauri for the last fifty years, so really, this one's on humanity for not checking. Your pipeline is in a similar position right now. AI's bypass got built. Everything downstream is finding out what that costs.
-->

---
# S13 - three views on AI ROI
layout: default
---

# Three reports, three conclusions

<div class="wwt-threeviews">
  <v-clicks>
    <div class="wwt-threeviews__card">
      <h3>Google Cloud</h3>
      <p class="wwt-threeviews__tag">Optimistic</p>
      <p>78% of executives have seen ROI on at least one use case.</p>
    </div>
    <div class="wwt-threeviews__card">
      <h3>Stanford AI Index</h3>
      <p class="wwt-threeviews__tag">Neutral</p>
      <p>Adoption is everywhere. Real structural change is rare.</p>
    </div>
    <div class="wwt-threeviews__card">
      <h3>MIT NANDA</h3>
      <p class="wwt-threeviews__tag">Pessimistic</p>
      <p>The shadow AI economy - official tools underdeliver, people route around them.</p>
    </div>
  </v-clicks>
</div>

<style>
.wwt-threeviews {
  flex: 1;
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 1.5rem;
}
.wwt-threeviews__card {
  padding: 1.5rem;
  border-radius: 12px;
  background: var(--wwt-primary-lightest);
}
.wwt-threeviews__tag {
  font-weight: 700;
  color: var(--wwt-secondary-base);
  text-transform: uppercase;
  font-size: var(--wwt-text-caption);
  letter-spacing: 0.04em;
  /* Dark-mode override lives in global-top.vue's unscoped style block:
     :global(.dark) here compiles to nothing in this project's Slidev/Vue
     toolchain - confirmed via computed style, not just visually - the same
     failure found in JCurve.vue/McpSchematic.vue/BarChart.vue. See that file
     for the fix and the full explanation. --wwt-primary-lightest flips to a
     near-transparent dark tint in dark mode (tokens.css) but
     --wwt-secondary-base does not, dropping this pairing to ~1.05:1. */
}
</style>

<!--
There isn't a consensus yet on what AI adoption is actually returning. Three sources, three different conclusions. [click]Google Cloud's 2025 report on AI ROI says 78% of executives have seen a return on at least one use case - we'll acll that the optimistic read. [click]Stanford's AI Index says adoption is everywhere but real structural change is rare, and most companies land somewhere flat - this is the neutral read. [click]We also have MIT's NANDA project describes what they call the shadow AI economy: official tools underdelivering while employees quietly route around them using unauthorized consumer apps - the pessimistic read.

All three are true at once, which tells you the interesting question isn't whether AI works. It's for whom, and under what conditions. It also highlights the importance of who you are asking. On one end of the spectrum, you have executives who aren't actively using the tools saying they are amazing. On the other end, developers who are expected to use these tools daily are reporting they aren't as effective as advertised.
-->

---
# S14 - the DORA credibility caveat (must read candid, not combative)
# v8 correction: previously implied a second study had already found a
# problem with the AI-adoption/performance correlation. Stride's study is
# pre-registered, not published - corrected below. Body also de-scripted:
# was verbatim first-person speaker prose; terse fragments per the outline.
layout: default
---

# Worth naming honestly

DORA's 2026 ROI report has taken real criticism this year - some call it
more brochure than research.

<p class="wwt-caveat__note">
Separately: Stride Research has <strong>pre-registered</strong> a study
testing whether AI adoption still correlates with performance once you
control for company size. Results expected later this year - not published
yet.
</p>

<style>
.wwt-caveat__note {
  font-size: var(--wwt-text-caption);
  color: var(--wwt-ink-muted);
}
</style>

<!--
Before I lean further on DORA's numbers, I want to flag something, because I'd rather you hear it now than lose trust in the rest of this talk later.

One detailed teardown called it more brochure than research, pointing out that the J-curve model is heavy on narrative and light on new primary data, and noting that DORA's research partner has openly said they wanted to reframe what they'd previously called an anomaly in their own 2024 numbers.

Separately, a research group called Stride has pre-registered a study specifically testing whether the correlation between AI adoption and elite performance survives controls for company size and existing engineering maturity - in other words, whether good teams are just good at everything, AI included, rather than AI being what makes them good. Worth being precise about where that actually stands: the methodology is public, the hypothesis is locked in, and the results are expected later this year. We don't have the answer yet. Neither does anyone else currently claiming to.

I think that's a fair challenge, and I'm not going to pretend it isn't out there.
-->

---
# S15 - pivot line (supporting principle, smaller than S36/S51)
# 2026-08-07: default quote text moved from 36px to 44px (global-top.vue),
# matching S51's size - so this line's override moved from 40px to 48px to
# hold the same relative gap and keep reading as a step up, not a step down:
# above the new 44px default, still below S36's 56px.
layout: quote
---

Be as skeptical of the AI-optimism industrial complex as you are of the
AI-hype industrial complex.

<style>
:deep(.wwt-quote__text) {
  font-size: 48px !important;
}
</style>

<!--
So take this as a rule for the rest of the talk: be exactly as skeptical of the people selling you AI optimism as you are of the people selling you AI hype. Both have a product.
-->

---
# S15b - Atlassian corroboration (v8: NEW). Same candid register as S14,
# not a chart - a third source alongside CircleCI and Sonar.
layout: default
---

# It isn't just DORA

Atlassian, independently this year: the **AI efficiency paradox** -
individual output speeds up, then backs up at review and approval.

<p class="wwt-caveat__note">Different company, different survey. No connection to DORA.</p>

<style>
.wwt-caveat__note {
  font-size: var(--wwt-text-caption);
  color: var(--wwt-ink-muted);
}
</style>

<!--
None of what matters here rests on DORA alone. Atlassian ran its own workforce research this year, independently, and landed on something they're calling an AI efficiency paradox: individual output speeds up, then piles up at review and approval, and most of the gain disappears before it ever reaches the system level. Different company, different survey, no connection to DORA at all, and it's the same mechanism this talk keeps coming back to. Watch what happens when we check the story again against people who weren't running a survey in the first place.
-->

---
# S16 - CircleCI scale stat
# title added: stats.vue only renders an <h1> when title is set - this
# slide had none, so it carried no heading landmark at all for screen-
# reader "jump by heading" navigation (WCAG 1.3.1/2.4.6).
layout: stats
title: Not a survey
---

<Stat
  value="28,000,000"
  label="CI workflows analyzed"
  caption="CircleCI 2026 - not a survey, actual pipeline data"
/>

<!--
CircleCI didn't ask anyone what they believed. They pulled 28 million actual CI workflow runs from thousands of real teams and looked at what happened.
-->

---
# S17 - throughput growth by percentile
layout: default
---

# Throughput growth by percentile

<BarChart
  :groups="[
    { label: 'Top 5%', bars: [{ value: 97, tone: 'primary' }] },
    { label: 'Top 10%', bars: [{ value: 47, tone: 'primary' }] },
    { label: 'Top 25%', bars: [{ value: 25, tone: 'primary' }] },
    { label: 'Median', bars: [{ value: 4, tone: 'secondary' }] },
    { label: 'Bottom 25%', bars: [{ value: 0, tone: 'flat', display: 'flat' }] },
  ]"
  unit="%"
  source="CircleCI 2026"
/>

<!--
Throughput was up 59% year over year, but that average is hiding the entire story. The top 5% of teams nearly doubled their output. The top 10% grew 47%. The top quarter grew 25%. The median team grew 4%. The bottom quarter grew nothing at all.
-->

---
# S18 - MUST-NAIL: feature vs. main branch by percentile
layout: default
---

# The bottleneck moved

<BarChart
  :groups="[
    { label: 'Median', bars: [{ value: 15, tone: 'secondary' }, { value: -7, tone: 'negative' }] },
    { label: 'Top 10%', bars: [{ value: 50, tone: 'secondary' }, { value: 0, tone: 'flat', display: 'flat' }] },
    { label: 'Top 5%', bars: [{ value: 85, tone: 'secondary' }, { value: 26, tone: 'primary' }] },
  ]"
  :legend="[{ label: 'Feature branch', tone: 'secondary' }, { label: 'Main branch', tone: 'primary' }]"
  unit="%"
  source="CircleCI 2026"
/>

<!--
Here's the number that made me stop and reread the chart. CircleCI split this by branch type. On feature branches - where people prototype and experiment - throughput rose for almost everyone, including a 15% bump for the median team and a 50% bump for the top decile. But on the main branch, the branch that actually ships to customers, the median team's throughput fell 7%. The top decile was flat. Only the top 5% managed to grow on both, gaining 26% on main branch as well.

Sit with that for a second. Developers across the industry are writing more code than they ever have. Almost none of it is reaching production. Activity is up. Delivery isn't. The old bottleneck was how fast someone could type. That bottleneck is gone. What's replaced it is integration, review, and recovery - the verification tax, showing up in raw pipeline data with no survey involved anywhere.

The bottleneck moved. [pause]
-->

---
# S19 - MTTR stat
# title added: see S16's note on stats.vue's heading gap.
layout: stats
title: Slower at fixing what breaks
---

<Stat
  value="72 min"
  label="Median time to recover"
  caption="+13% YoY · feature branches: 80 min (+25%) - CircleCI 2026"
/>

<!--
Median time to recover from a failed build [click]is now 72 minutes, up 13% from last year. On feature branches it's closer to 80 minutes, up 25%. We're getting slower at fixing what breaks, at exactly the moment we're breaking more of it.
-->

---
# S20 - main-branch success rate, now a two-step trend (v8: updated data)
# stats.vue wraps items in <v-clicks>, so the two values build automatically
# - no custom CSS needed for the reveal.
layout: stats
title: Main-branch success rate
stats:
  - value: "70.8%"
    label: September 2025
    caption: "Lowest in 5+ years · benchmark is 90%"
  - value: "76.7%"
    label: March 2026
    caption: "Improving - still short of the 2023–24 mid-80s and the 90% benchmark"
---

<!--
And main-branch success rate [click]had dropped to 70.8% as of last September, the lowest it had been in five years, against an industry benchmark of 90%. Roughly three in ten attempts to merge into production code were failing outright.

[click] A follow-up check-in CircleCI published just a few weeks ago, using fresher data from this past March, shows that number climbing back to 76.7%. Genuinely encouraging. Still nowhere near the mid-80s teams were hitting back in 2023 and 2024, and still well short of that 90% benchmark.
-->

---
# S20b - Merge Efficiency Ratio (v8: NEW)
layout: stats
title: "Merge Efficiency Ratio - validation cycles per merged change"
stats:
  - value: "3.9"
    label: Median team
  - value: "2.6"
    label: Top 5%
  - value: "1.3"
    label: Elite cohort
    caption: "20 organizations · CircleCI, July 2026"
---

<!--
That same follow-up report introduced a metric worth stealing for your own dashboards: [click]merge efficiency ratio, or MER, which counts how many validation cycles it actually takes to get one change onto the main branch. It's a direct read on how much rework is hiding behind whatever your throughput number says. The median team runs at an MER of about 3.9. [click]The top 5% run at 2.6. [click]A small cohort of twenty elite organizations in the same dataset run at 1.3, and improved on that by 21% in a single year. If you take away one new number to start tracking after this talk, that's a strong candidate.
-->

---
# S21 - the 12-FTE math, built line by line. ALTERNATE, hidden: S21b
# (dollar-cost framing) is live instead. Swap `hide: true` between S21 and
# S21b to re-frame for the room - FTEs for an engineering audience, dollars
# for a budget-holding one. Not both-and (per v8 outline).
layout: default
hide: true
---

# The 12-FTE math

<div class="wwt-ftemath">
<v-clicks>
<p>At 500 changes a day...</p>
<p>...a drop from <strong>90% → 70%</strong> success rate...</p>
<p class="wwt-ftemath__result">...is equivalent to <strong>12 FTEs lost.</strong></p>
<p class="wwt-ftemath__note">Not added. Lost.</p>
</v-clicks>
</div>

<p class="wwt-ftemath__source">Modeled from CircleCI 2026 success-rate and MTTR data.</p>

<style>
.wwt-ftemath__source {
  font-size: var(--wwt-text-caption);
  color: var(--wwt-ink-muted);
}
.wwt-ftemath {
  flex: 1;
  display: flex;
  flex-direction: column;
  justify-content: center;
  gap: 1rem;
  font-size: var(--wwt-text-h2);
}
.wwt-ftemath__result {
  font-size: 40px;
  font-weight: 700;
  color: var(--wwt-accent6-base);
}
.wwt-ftemath__note {
  color: var(--wwt-ink-muted);
  font-size: var(--wwt-text-body);
}
</style>

<!--
Do the arithmetic on that and it gets uncomfortable fast. A team pushing five changes a day that drops from 90% to 70% success, at a 60-minute average recovery time, loses about 250 hours a year to debugging and blocked releases.

[click] Scale that to 500 changes a day, which is where CircleCI's top ten teams actually live, and the same drop in success rate is equivalent to

[click] losing twelve full-time engineers.

[click] Not hiring twelve fewer. Losing twelve you already had, to friction that never shows up on a headcount spreadsheet.
-->

---
# S21b - dollar-cost framing (v8: NEW). Live alternate to S21 - not both-and.
layout: stats
title: "50-developer team, ~3,000 changes a month"
stats:
  - value: "$900K"
    label: a year at risk
    caption: "Unoptimized workflows - agents idling until their context cache expires"
  - value: "$700K+"
    label: recoverable
    caption: "By moving routine validation earlier - CircleCI, July 2026"
---

<style>
/* Red-to-blue shift, not red-to-green - the palette has no green
   (tokens.css) and Stat.vue has no tone prop, so its value is always
   --wwt-primary-base by default. :deep() required: a bare .wwt-stat__value
   selector compiles to a stamped attribute that never matches. */
:deep(.wwt-stat:first-child .wwt-stat__value) {
  color: var(--wwt-accent6-base);
}
</style>

<!--
If FTEs feel a little abstract, here's the same story in dollars instead. [click]That same recent CircleCI report modeled a fifty-developer team shipping about three thousand changes a month at a fairly ordinary MER. Run that team through unoptimized, agent-heavy workflows with slow CI feedback and the friction costs something like nine hundred thousand dollars a year, a lot of it from agents sitting idle waiting for a green light long enough that their context cache expires and they have to start over from scratch. [click]Move the routine checks earlier, into the loop where the code actually gets written instead of after it's already been pushed, and that same team can claw back seven hundred thousand dollars or more of it. FTEs or dollars, take whichever framing lands better with whoever controls your budget.
-->

---
# S22 - vanity metric line
layout: quote
---

Throughput is a vanity metric. The real metric is integrations that ship.

<!--
Which gets us to the line I actually want you to remember from this section: if the only thing you're measuring is developer velocity, you've already lost the plot.
-->

---
# S23 - tokenmaxxing, three-panel build (comic timing, panel 3 reverses)
layout: default
---

# Tokenmaxxing

<div class="wwt-tokenmax">
<v-clicks>
<div class="wwt-tokenmax__panel">
<p class="wwt-tokenmax__label">Claudeonomics leaderboard - "Token Legend" tier</p>
<h3>Meta: 85,000 employees. 60 trillion tokens. One month.</h3>
</div>
<div class="wwt-tokenmax__panel">
<h3>Uber: entire 2026 AI budget gone. Month 4 of 12.</h3>
</div>
<div class="wwt-tokenmax__panel wwt-tokenmax__panel--reversal">
<h3>Amazon, weeks later: leaderboard deleted.</h3>
<p>"Don't use AI just to use AI."</p>
</div>
</v-clicks>
</div>

<p class="wwt-tokenmax__source">Reported across multiple industry outlets, 2026 - figures not independently verified.</p>

<style>
.wwt-tokenmax__source {
  font-size: var(--wwt-text-caption);
  color: var(--wwt-ink-muted);
}
.wwt-tokenmax {
  flex: 1;
  display: flex;
  flex-direction: column;
  justify-content: center;
  gap: 1.25rem;
}
.wwt-tokenmax__panel {
  padding: 1.25rem 1.5rem;
  border-radius: 12px;
  background: var(--wwt-primary-lightest);
}
.wwt-tokenmax__panel h3 {
  margin: 0;
}
.wwt-tokenmax__label {
  margin: 0 0 0.25rem;
  font-size: var(--wwt-text-caption);
  color: var(--wwt-ink-muted);
}
.wwt-tokenmax__panel--reversal {
  background: var(--wwt-secondary-base);
  color: var(--wwt-ink-white);
}
</style>

<!--
I want to close this part of the talk with what might be the cleanest real-world example of that mistake I found anywhere this year, and it's happening right now, not in a case study from three years ago.

Earlier this year, several large tech companies started building internal leaderboards that rank employees by how many AI tokens they burn through. This now has a name: tokenmaxxing.

[click] Meta reportedly ran one of these across 85,000 employees, with tier names like Session Immortal and Token Legend, and hit 60 trillion tokens of consumption in a single month.

[click] Uber, separately, burned through its entire annual AI budget in four months.

You can guess the ending. Measuring tokens consumed without measuring what shipped is like judging a factory by its electric bill instead of what came off the line. [click]Amazon killed its own internal leaderboard shortly after, reportedly under the internal line "don't use AI just to use AI."

That's the vanity-metric mistake from earlier in this section, playing out at trillion-token scale, at three of the most sophisticated tech companies on the planet, this year.
-->

---
# S24 - Section 3 opener ("Section 3: Paying the Verification Tax").
# Hidden (v8 merge, 2026-07-31) - mid-talk section dividers cut for pacing.
# Its speaker-note opener (the Babel fish beat) moved to the top of S25's
# notes below so the Hitchhiker callback survives the hide.
#
# Jeremy
layout: section
number: "03"
title: The Babel Fish Problem
hide: true
---

<!--
Quick detour into the Babel fish. In the Hitchhiker's Guide, it's a small yellow creature you stick in your ear that translates any language into your own, instantly. Genuinely useful. Also, memorably, used in the book as a proof against the existence of God - the argument being that something this convenient couldn't have arisen naturally, so its existence disproves the faith required to believe in a creator. It's absurd and also kind of airtight, which is the bar I'm setting for what follows.
-->

---
# S25 - trust gap stat
layout: stats
title: The gap is where incidents come from
stats:
  - value: "96%"
    label: don't fully trust AI code
  - value: "48%"
    label: always verify before committing
    caption: Sonar 2026, n=1,149 developers
---

<!--
Quick detour into the Babel fish. In the Hitchhiker's Guide, it's a small yellow creature you stick in your ear that translates any language into your own, instantly. Genuinely useful. Also, memorably, used in the book as a proof against the existence of God - the argument being that something this convenient couldn't have arisen naturally, so its existence disproves the faith required to believe in a creator. It's absurd and also kind of airtight, which is the bar I'm setting for what follows.

Why does verification cost so much? [click]Sonar asked 1,149 developers and found that 96% don't fully trust that AI-generated code is functionally correct. [click]And yet only 48% of them always check it before committing. Ninety-six percent distrust, forty-eight percent verification - that gap is roughly where your production incidents live.
-->

---
# S26 - "looks correct but isn't reliable"
layout: quote
attribution: 61% of developers
role: Sonar, 2026
---

Code that looks correct but isn't reliable.

<!--
Sixty-one percent of developers put a specific name to the failure mode: code that looks correct but isn't reliable. This is a different kind of bug than what a junior developer typically produces. Junior mistakes tend to be loud - a stack trace, a compile error. AI mistakes tend to be quiet. The code compiles. The tests pass. The logic reads fine on a first pass. Then three weeks later it does the wrong thing under load.
-->

---
# S27 - the METR reveal: predicted → felt → measured
layout: default
---

# Predicted, felt, measured

<div class="wwt-metr">
<v-clicks>
<p>Predicted: <strong>+24% faster</strong></p>
<p>Felt: <strong>+20% faster</strong></p>
<p class="wwt-metr__reveal">Measured: <strong>19% slower</strong></p>
</v-clicks>
</div>

<p class="wwt-metr__source">METR, randomized controlled trial, July 2025 - 16 developers, 246 real tasks.</p>

<style>
.wwt-metr {
  flex: 1;
  display: flex;
  flex-direction: column;
  justify-content: center;
  gap: 1rem;
  font-size: var(--wwt-text-h2);
}
.wwt-metr__reveal {
  font-size: 48px;
  font-weight: 700;
  color: var(--wwt-accent6-base);
}
.wwt-metr__source {
  font-size: var(--wwt-text-caption);
  color: var(--wwt-ink-muted);
}
</style>

<!--
In mid-2025, a nonprofit called METR ran an actual randomized controlled trial on AI coding productivity - the same basic design used in drug trials. Sixteen experienced open-source developers, 246 real tasks, in codebases they'd already worked in for years. Each task got randomly assigned to either "AI allowed" or "AI not allowed."

[click]Before the study started, developers predicted AI would make them 24% faster.

[click] After finishing, they believed they'd been about 20% faster.

[click] The measured result: 19% slower. [pause]

Predicted faster. Felt faster. Measured slower. If developers can be that wrong about their own experience using a tool, it's worth wondering how much we should trust anyone's gut feeling about whether AI is helping at the organizational level. Don't trust the sensation of speed - measure what actually happened.
-->

---
# S28 - METR epilogue (first cut candidate if running long)
layout: default
---

# 2026: they tried to re-run it

METR tried to re-run the study in early 2026 to see whether newer tools
changed the result - and the study itself broke.

Too many developers refused the "no AI" condition, even for pay. There was
no longer a clean control group left to recruit.

<!--
People had become dependent enough on the tool that you literally couldn't recruit a control group to measure life without it. I'm not going to hand you a corrected percentage, because I don't think anyone honest can give you one yet. The picture got muddier, not clearer. Notice what that failure actually is: it's the trust gap and the verification tax showing up inside the methodology of the people trying to study them.

The mental model I'd take from all of this is the drafting mindset. AI proposes. A human disposes.
-->

---
# S29 - DORA 2025 AI effects. Bridge beat now written into notes below.
# Hidden for the 40-min delivery (v8 merge, 2026-07-31) - deck already carries
# this argument via Sonar (S25/26) and the METR RCT (S27/28). Un-hide by
# deleting the hide: line if building a longer version of the talk.
layout: default
hide: true
---

# What AI actually changed

<BarChart
  orientation="horizontal"
  :groups="[
    { label: 'Code quality', bars: [{ value: 2, tone: 'primary', display: 'improved' }] },
    { label: 'Documentation quality', bars: [{ value: 2, tone: 'primary', display: 'improved' }] },
    { label: 'Review speed', bars: [{ value: 2, tone: 'primary', display: 'improved' }] },
    { label: 'Individual productivity', bars: [{ value: 3, tone: 'primary', display: 'improved' }] },
    { label: 'Team performance', bars: [{ value: 2, tone: 'primary', display: 'improved' }] },
    { label: 'Product performance', bars: [{ value: 2, tone: 'primary', display: 'improved' }] },
    { label: 'Organizational performance', bars: [{ value: 2, tone: 'primary', display: 'improved' }] },
    { label: 'Delivery instability', bars: [{ value: 1, tone: 'negative', display: 'worse' }] },
    { label: 'Job satisfaction / burnout', bars: [{ value: 1, tone: 'flat', display: 'flat' }] },
    { label: 'Friction in the dev process', bars: [{ value: 1, tone: 'flat', display: 'flat' }] },
  ]"
  source="DORA 2025, n=5,000 - directional, not exact effect sizes."
/>

<!--
DORA's own effects data tells a similar story to what we've just walked through: mostly positive, three flagged. Individual productivity, team and product performance, code and documentation quality, review speed - all up. But delivery instability is up too, and job satisfaction and process friction are flat, not improved. Same shape as everything else in this section: the gains are real, and they don't come free.

NOTE (2026-07-31): hidden for this delivery - see hide: true below. Un-hide by deleting that line, but re-verify these effect sizes against import/research/DORA_State_of_AI-Assisted_Software_Development_2025.pdf first (TODO carried over from before: never independently confirmed against the source report).
-->

---
# S30 - AI proposes, humans dispose
layout: quote
role: Every AI output is a junior dev's first PR.
---

AI proposes. Humans dispose.

<style>
/* No attribution: set - quote.vue renders that span regardless (no v-if),
   so its empty content leaves a phantom line-height gap above role. Collapse it. */
:deep(.wwt-quote__attribution:empty) {
  display: none;
}
</style>

<!--
Every piece of AI output is a first draft from a junior engineer - not because the model is unintelligent, sometimes it's quite good, but because you're the one accountable for what ships, and the model will not be in the room for the post-incident review.
-->

---
# S31 - Section 4 opener ("Section 4: Where You Actually Are").
# Hidden (v8 merge, 2026-07-31) - mid-talk section dividers cut for pacing.
# Its speaker-note opener (the towel beat) moved to the top of S32's notes
# below so the Hitchhiker callback survives the hide.
#
# PJ
layout: section
number: "04"
title: Where Your Towel Won't Save You
hide: true
---

<!--
The towel gets described in the Hitchhiker's Guide as the single most useful thing a hitchhiker can carry. That's true right up until it isn't, and a fair number of you are currently standing in a situation the towel can't help with. You may not have noticed yet. Let's find out.

DORA took their 5,000 survey respondents and statistically sorted them into seven team archetypes. I'll go through all seven quickly. You're in here somewhere.
-->

---
# S32 - MUST-NAIL: seven archetypes, six visual groupings (holds ~2.5 min)
layout: default
---

# Seven archetypes, six groups

<ArchetypeGrid source="DORA 2025, n=5,000" />

<!--
Cluster one, foundational challenges, 10%: stuck in survival mode, burnout high, most metrics low. If your standup routinely opens with "okay, what's actually on fire today," this is probably you. Deploying agents here just makes the fire bigger. Fix the standup first.

Cluster two, the legacy bottleneck, 11%: reactive by default, unstable systems dictating the pace of everything. Handing AI to a team in this state is a bit like responding to a kitchen fire by reading up on kerosene.

Cluster three, constrained by process, 17%: running hard and getting nowhere, because the systems are fine but the process eats every hour available. AI helps the individual, but the process drag absorbs whatever it gains you.

Clusters four and five sit in the middle - high impact but low cadence at 7%, and stable and methodical at 15% - both doing solid work at a moderate pace with generally low burnout. AI tends to be a real multiplier for both once the surrounding infrastructure can keep up.

Cluster six, pragmatic performers, 20%, the single largest group: fast, stable, well-being about average. Mostly harmless, in the best sense.

Cluster seven, harmonious high-achievers, 20%: strong on every dimension, including well-being. These are the teams showing up in CircleCI's top 5%. AI is making them richer, and they are also, not coincidentally, your competition.

Add it up: 38% of teams sit in real difficulty, clusters one through three. 40% are in genuinely good shape, clusters six and seven. If you honestly can't tell which group you're in, that itself is a signal.
-->

---
# S33 - 38% / 40% split stat
# title added: see S16's note on stats.vue's heading gap.
layout: stats
title: Add it up
stats:
  - value: "38%"
    label: in clear difficulty
    caption: clusters C1 + C2 + C3
  - value: "40%"
    label: high-performing
    caption: clusters C6 + C7 - DORA 2025, n=5,000
---

<!--
[click]Two patterns push teams toward the worse end faster [click]than they'd otherwise drift there on their own.
-->

---
# S34 - the shadow AI economy stat
layout: stats
title: The shadow AI economy
stats:
  - value: "4"
    label: AI tools per team
  - value: "35%"
    label: access via personal accounts
    caption: ChatGPT 52% · Perplexity 63% - Sonar 2026
---

<!--
The first is what I'd call BYOAI culture. [click]The average team now runs four different AI tools, and plenty of that use isn't sanctioned. [click]Thirty-five percent of developers access AI through personal accounts rather than company ones - 52% for ChatGPT specifically, 63% for Perplexity. This is the shadow AI economy MIT NANDA described. The fix isn't banning personal accounts. It's making the sanctioned option genuinely better than the consumer one.
-->

---
# S35 - juniors vs. seniors
layout: comparison
title: The experience gap - Sonar 2026
left:
  title: Juniors
  points:
    - "+40% productivity gain"
    - "66% report \"looks correct but isn't reliable\""
right:
  title: Seniors
  points:
    - "+32% productivity gain"
    - "48% report \"looks correct but isn't reliable\""
---

<!--
The experience gap. Junior developers report a 40% productivity gain from AI, compared to 32% for seniors, but juniors also report the "looks correct but isn't reliable" failure mode at 66%, against 48% for seniors. The takeaway isn't that junior developers should use AI less. It's that senior review needs to be sitting on top of AI-generated code specifically.

[pause] Here's the line I want you to actually write down.
-->

---
# S36 - MUST-NAIL ANCHOR: largest treatment in the deck
layout: quote
dark: true
---

AI cannot rescue an unhealthy platform.

<style>
:deep(.wwt-quote__text) {
  font-size: 56px !important;
}
</style>

<!--
AI cannot rescue an unhealthy platform. [pause] [say it again]

I want to give you a real example of that sentence, because it happened this year, at a company with more infrastructure sophistication than almost anyone else building software.
-->

---
# S37 - MUST-NAIL: Amazon Kiro timeline (v8: 4 events, specific figures).
# Tone: sober and factual - the point is "this happens to the best," not a
# dunk. timeline.vue has no body slot, so the Amazon-dispute note attaches to
# the Mar 5 event's own detail rather than floating as a separate footnote.
# grid-auto-flow: column means 4 events = 4 equal columns. At 4 columns the
# title's "(public reporting, 2025-2026)" qualifier plus long labels/details
# genuinely overflowed past the footer at 1920x1080 (confirmed visually,
# 2026-07-31) - trimmed the title to one line and shortened the Dec-2025 and
# Amazon's-fix label/detail pairs to recover the vertical budget. The
# "public reporting" framing moved to the speaker notes below; the real
# dates on each event already carry the timeframe on screen.
layout: timeline
title: "If it can happen there..."
events:
  - date: Dec 2025
    label: Elevated permissions
    detail: Bypasses two-person approval, deletes and rebuilds the environment. 13-hour outage.
  - date: "Mar 2, 2026"
    label: AI-assisted changes
    detail: 120,000 lost orders. 1.6 million website errors.
  - date: "Mar 5, 2026"
    label: Worse
    detail: "~6.3M lost orders, a 99% drop in North American order volume, ~6 hours down. Amazon disputes AI as the primary cause."
  - date: "Amazon's fix"
    label: Mandatory review, reinstated
    detail: Senior-engineer sign-off on AI changes. A human, back in the loop.
---

<!--
In December of 2025, one of Amazon's internal AI agents - a system called Kiro built to handle operational and coding work autonomously - was pointed at a production issue. It decided the right move was to delete the environment and rebuild it from scratch. [click]It was operating under an engineer's elevated permissions, so the standard two-person approval that would normally gate a change of that size simply didn't apply. The result: a thirteen-hour outage.

[click]Three months later, in March 2026, Amazon had a genuinely bad week, and this time there are real numbers attached to it. On March 2nd, an incident tied to AI-assisted changes caused 120,000 lost orders and 1.6 million website errors. [click]Three days later, on March 5th, a second and considerably worse incident took the U.S. site down for roughly six hours, dropping North American order volume by 99% and costing something like 6.3 million lost orders. Amazon disputes that AI was the primary cause of at least one of these and has called it user error, which I think is worth including rather than leaving out, because it's a fair point to raise even though it doesn't change what happened. Internal documentation reportedly described the broader pattern going back to the previous fall as a trend of incidents connected to Gen-AI-assisted changes.

[click]Here's the part worth actually remembering, though. Amazon's response was to reinstate mandatory senior-engineer review specifically for AI-assisted changes. They put a human back in the loop, because an agent had been handed disposal authority instead of proposal authority, and nothing downstream was strong enough to catch it before it mattered.

This is Amazon. If it can happen to a company with that much operational sophistication, "we're careful, it won't happen to us" isn't a strategy. It's a hope wearing a strategy's clothes.
-->

---
# S38 - bridge
layout: default
---

<div class="wwt-bridge">Amplifier works both directions.</div>

<style>
.wwt-bridge {
  flex: 1;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: var(--wwt-text-h1);
  font-weight: 300;
  color: var(--wwt-ink-muted);
}
</style>

<!--
AI cannot rescue an unhealthy platform. It turns out it can't always protect a healthy one either, if you hand it authority to act before you've built the verification to catch it when it's wrong.
-->

---
# S39 - Section 5 opener ("Section 5: How to Flatten the Curve").
# Hidden (v8 merge, 2026-07-31) - mid-talk section dividers cut for pacing.
# Its speaker-note opener moved to the top of S40's notes below so the beat
# survives the hide.
#
# PJ
layout: section
number: "05"
title: Build, but then Verify
hide: true
---

<!--
We've now looked at the dip from four angles: pipeline data, developer experience, a controlled experiment, and a thirteen-hour outage at one of the largest companies on earth. Let's talk about how the teams doing this well are actually flattening it.
-->

---
# S40 - vibe then verify
layout: quote
role: Generate fast. Validate harder.
---

(Vibe) Build, then verify.

<style>
/* Same empty-attribution gap as S30 - see that slide's note. */
:deep(.wwt-quote__attribution:empty) {
  display: none;
}
</style>

<!--
There's a phrase from Sonar's research that fits the whole answer into three words: vibe (which I hate the word as it doesn't really apply - so will use build), then verify. Generate quickly, then check harder than you're currently checking.
-->

---
# S41 - self-healing tests, before/after
layout: comparison
title: Self-healing test automation (vendor-reported figures)
left:
  title: Before
  points:
    - Coverage 30–50%
    - Authoring takes days to weeks
    - Payback in 12–24 months
right:
  title: After
  points:
    - Coverage 80–95%
    - Authoring takes hours
    - Payback in ~3 months
---

<!--
Self-healing test automation addresses the first place the verification tax usually bites: AI writes more code, the test suite breaks more often trying to keep up, teams start quietly disabling flaky tests, and coverage rots from there. Self-healing tests use dynamic baselining and predictive change detection to recover automatically instead of failing loudly. Treat any specific vendor claim with the usual skepticism, but the direction here is not in question.
-->

---
# S42 - progressive delivery flow
layout: default
---

# Progressive delivery

<ProgressiveDelivery />

<!--
Progressive delivery with automatic rollback is the cleanest real-world example of "over the loop" I can offer you, and there's a line from the Guide that describes it better than I could: the knack of flying, Adams writes, lies in learning how to throw yourself at the ground and miss. That's more or less what this pattern does on purpose. Push a change to a small slice of traffic, let real-time telemetry make the call, and if error budgets or p99 latency get blown, roll back automatically before anyone gets paged. Remember that 72-minute median recovery time from earlier? This turns it into seconds.
-->

---
# S43 - verification layer stat
# title added: see S16's note on stats.vue's heading gap.
layout: stats
title: The cost of skipping it
stats:
  - value: "+80%"
    label: more outages, without a verification layer
  - value: "+46%"
    label: more quality impact, without a verification layer
    caption: Non-users vs. users of a deterministic verification tool - Sonar 2026
---

<!--
A real verification layer matters more than it sounds like it should. [click]Sonar found that teams without a deterministic verification tool were 80% more likely to report increased outage frequency tied to AI adoption, [click]and 46% more likely to say AI had hurt their code quality overall. That layer buys you paying the verification tax once, automatically, instead of paying it repeatedly in every code review.
-->

---
# S44 - Coinbase contrast (second cut candidate if running long)
layout: default
---

# The positive contrast

Coinbase restructured its engineering interviews around directing and
verifying AI output - not writing code from scratch.

<!--
Same underlying bet as everything else in this section, just made early and on purpose instead of learned the hard way in a postmortem.
-->

---
# S45 - DORA AI Capabilities Model
# NOT `layout: process`: that layout's steps grid uses fixed equal-width
# columns with no wrapping, and empirically overflows past the slide edge
# at 6+ items (confirmed by rendering it - only 5 of 6 columns are visible,
# regardless of how short the content is). A custom 4x2 grid on `default`
# fits all seven without cutting any real content (7 items in an 8-cell
# grid; last cell intentionally empty).
layout: default
---

# The AI Capabilities Model - DORA 2025

<div class="wwt-capabilities">
  <div class="wwt-capabilities__item">
    <span class="wwt-capabilities__num">01</span>
    <h3>Clear AI stance</h3>
    <p>A stated, communicated position on AI use.</p>
  </div>
  <div class="wwt-capabilities__item">
    <span class="wwt-capabilities__num">02</span>
    <h3>Healthy data ecosystems</h3>
    <p>Data that's well-governed at the source.</p>
  </div>
  <div class="wwt-capabilities__item">
    <span class="wwt-capabilities__num">03</span>
    <h3>AI-accessible data</h3>
    <p>Internal data AI tooling can actually reach.</p>
  </div>
  <div class="wwt-capabilities__item">
    <span class="wwt-capabilities__num">05</span>
    <h3>Small batches</h3>
    <p>Working in small, reviewable increments.</p>
  </div>
  <div class="wwt-capabilities__item">
    <span class="wwt-capabilities__num">06</span>
    <h3>User-centric focus</h3>
    <p>Genuinely centered on the user, not the roadmap.</p>
  </div>
  <div class="wwt-capabilities__item">
    <span class="wwt-capabilities__num">07</span>
    <h3>Quality internal platforms</h3>
    <p>Platforms worth building on.</p>
  </div>
</div>

<style>
.wwt-capabilities {
  flex: 1;
  min-height: 0;
  overflow: hidden;
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-template-rows: repeat(2, 1fr);
  gap: 0.75rem 1.5rem;
}
.wwt-capabilities__item {
  min-height: 0;
  padding: 0.75rem 1.25rem 0;
  border-top: 4px solid var(--wwt-primary-base);
  display: flex;
  flex-direction: column;
  gap: 0.25rem;
}
.wwt-capabilities__num {
  font-size: 22px;
  font-weight: 300;
  color: var(--wwt-primary-base);
  line-height: 1;
}
.wwt-capabilities__item h3 {
  font-size: 19px;
  font-weight: 600;
  margin: 0;
}
.wwt-capabilities__item p {
  font-size: 13px;
  line-height: 1.35;
  color: var(--wwt-ink-muted);
  margin: 0;
}
</style>

<!--
RESOLVED (2026-07-31): confirmed against the DORA 2025 source PDF (report pp. 49-50, Figure 45 on p. 62) - the model is seven capabilities, and "healthy, AI-accessible data" was two source capabilities collapsed into one. Split them back apart above (items 02/03). All seven now named; nothing padded.
-->

<!--
Underneath all three of those tactics sits one strategic framework worth knowing: DORA's AI Capabilities Model, seven practices that specifically amplify whatever benefit AI provides. None of this is new - DORA's been making most of this argument for a decade - but what is new is the finding that these specific practices are what make AI adoption pay off. Which is why the answer to "how do we get ROI from AI" turns out to be identical to "how do we run a good engineering organization." Successful AI adoption is a systems problem, not a tools problem.
-->

---
# S46 - MCP schematic (trimmed vs v1, fewer annotations)
layout: default
---

# One protocol, any cloud

<McpSchematic />

<!--
The architecture that ties all of this together across different clouds is the Model Context Protocol, or MCP: a portable reasoning engine that talks to AWS, GCP, or Azure through one standardized interface instead of being locked into a single vendor's stack. Zero-trust, auditable, and genuinely portable. If you're an architect, it's worth a weekend of reading.
-->

---
# S47 - systems problem, not a tools problem
layout: quote
attribution: DORA, 2026
---

Successful AI adoption is a systems problem, not a tools problem.

<!--
Vibe, then verify. Generate fast. Check harder.
-->

---
# S48 - Closing opener
# number: dropped - matches S7; a titled break, not a numbered 6th section
# now that S12/S24/S31/S39 are hidden. See S7's note for the full rationale.
#
# Jeremy
layout: section
title: The Roadmap
---

<!--
Three concrete steps to take home.
-->

---
# S49 - three-step pilot playbook
layout: process
title: Three steps to take home
steps:
  - title: Pick a high-velocity, non-critical surface
    detail: Not billing, not auth. Internal tools, a dev dashboard. Put the J-curve dip somewhere it can't hurt you.
  - title: Instrument the right things
    detail: Main-branch throughput, success rate vs. a 90% benchmark, recovery time vs. 60 minutes - not lines of code. As we just saw, vanity metrics can hide the paradox at trillion-token scale.
  - title: Budget for the tuition, up front
    detail: Tell your CFO before the dip arrives that you expect one. Spend political capital holding the line, not explaining yourself after the fact.
---

<!--
[click]First, pick a high-velocity, non-critical surface to start on. The J-curve dip is real, so put it somewhere it can't actually hurt you.

[click]Second, instrument the right things. Not lines of code, not pull requests per developer - those metrics hide the paradox we've spent this whole talk describing, and as we just saw with tokenmaxxing, they can hide it at a genuinely spectacular scale.

[click]Third, budget for the tuition up front. Set the expectation early enough that when the dip actually shows up, you're spending your political capital holding the line instead of explaining yourself after the fact.
-->

---
# S49b - reinvest the payoff on purpose, not by accident
layout: comparison
title: Reinvest on purpose, not by accident
left:
  title: By accident
  points:
    - "The \"free headcount\" DORA describes gets absorbed into more feature pressure"
    - You stay the implementer
right:
  title: On purpose
  points:
    - Reinvested in quality, security, and paying down technical debt
    - You become the systems architect and quality governor
---

<!--
When the payoff arrives, reinvest it deliberately - DORA calls that freed-up capacity "free headcount," and it's yours to spend on quality, security, and technical debt, not more feature pressure. The role is shifting too: less typing, more judgment - a better job, honestly, as long as you treat the transition on purpose instead of letting it happen to you by accident.
-->

---
# S50 - code is a liability
layout: quote
attribution: Software Engineering at Google
role: via DORA 2026
---

Code is not an asset. Code is a liability.

<!--
Code isn't an asset, it's a liability - the cost of running software long-term dwarfs the cost of writing it in the first place.
-->

---
# S51 - most quotable line, biggest/cleanest treatment
layout: quote
dark: true
attribution: DORA, 2026
---

We don't measure AI by the code it writes, but by the bottlenecks it clears.

<style>
:deep(.wwt-quote__text) {
  font-size: 44px !important;
}
</style>

<!--
That's really the whole point. The agents are clearing out the parts of the job nobody wanted anyway - the brittle tests, the 2am rollback, the fourth pass through the same YAML file. The interesting work stays exactly where it was. You just get more time for it.
-->

---
# S52 - "DON'T PANIC" bookend (closes the talk)
layout: image
image: /images/dont-panic.png
backgroundSize: fill
hideBadge: true
---

<!--
Don't panic. We were told that much, in large friendly letters, by a much wiser book than this one. Build the verification layer first. Budget for the dip before it shows up. Vibe, then verify. And when you get home, tell your team the answer isn't 42.

The answer is: go clear the bottlenecks AI can't.
-->

---
layout: thank-you
speakers:
  - name: Jeremy Meiss
    socials:
      bluesky: jerdog.dev
      linkedin: /in/jeremy-meiss
      github: jerdog
      website: jmeiss.me
  # - name: PJ Hagerty
  #   socials:
  #     linkedin: /in/pj-hagerty
  #     github: pjhagerty
slidesUrl: https://github.com/jerdog/talk-dont-panic-agentic-devops
---

So long, and thanks for all the boilerplate.

<style>
:deep(.wwt-end__signoff) {
  font-size: 32px;
}
</style>

<!--
So long, and thanks for all the boilerplate.

Thank you.

[Q&A]
-->
