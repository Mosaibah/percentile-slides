---
# You can also start simply with 'default'
theme: seriph
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: https://cover.sli.dev
# some information about your slides (markdown enabled)
title: DevOps Response Time Metrics
subtitle: Why Percentiles Win
info: |
  ## DevOps Response Time Metrics
  An interactive presentation on why percentiles are superior to averages for understanding performance.

  Inspired by Slidev features!
# apply unocss classes to the current slide
class: text-center
# https://sli.dev/features/drawing
drawings:
  persist: false
# slide transition: https://sli.dev/guide/animations.html#slide-transitions
transition: slide-left
# enable MDC Syntax: https://sli.dev/features/mdc
mdc: true
highlighter: shiki
---

# DevOps Response Time Metrics
## Why Percentiles Win

<div class="abs-br m-6 text-xl">
  <button @click="$slidev.nav.openInEditor()" title="Open in Editor" class="slidev-icon-btn">
    <carbon:edit />
  </button>
  <a href="https://github.com/slidevjs/slidev" target="_blank" class="slidev-icon-btn">
    <carbon:logo-github />
  </a>
</div>

---
transition: fade-out
level: 1
---

# A Tale of Misleading Averages

Imagine you're monitoring an API. You get 10 requests with these response times (in milliseconds):

<div v-click="1">
  <div class="mt-4">
```json
[100, 120, 110, 130, 115, 125, 105, 140, 150, 2000]
```
  </div>
</div>

<div v-click="2">
  <div class="mt-4">
Average: <code v-mark.red.underline="3">(100 + 120 + 110 + 130 + 115 + 125 + 105 + 140 + 150 + 2000) / 10 =</code> **<code v-mark.red.bold="3">399.5ms</code>**
  </div>
</div>

<div v-click="4">
  <p class="mt-4">
Sounds bad, right? But <span v-mark.highlight.orange="5">look closer...</span>
<br>
Why this matters: One outlier (<code v-mark.orange="5">2000ms</code>) skews the average, making performance seem worse than it is. Let's dig deeper.
  </p>
</div>

<!--
Key points for this slide:
- Introduce the problem with a clear example.
- Use v-click to reveal information step-by-step.
- Use v-mark to highlight the outlier and the skewed average.
Clicks:
1: Show data
2: Show average calculation text
3: Highlight parts of average calculation
4: Show "Sounds bad..." text
5: Highlight parts of "Sounds bad..." text
-->

<style>
h1 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>

---
transition: slide-up
level: 1
---

# Sorting to See the Truth

Let's sort those response times:
<br>
<br>

````md magic-move
```json {all|all}
// Unsorted
[100, 120, 110, 130, 115, 125, 105, 140, 150, 2000]
```

```json {all|all}
// Sorted
[100, 105, 110, 115, 120, 125, 130, 140, 150, 2000]
```
````

<div v-click="1" class="mt-4">
Now, what if we want to know what 90% of users experience? Count 90% of the way through the sorted list (9th value): **<code v-mark.blue.bold="2">150ms</code>**.
<br>
That number? We call it the <span v-mark.underline.blue="2">90th percentile or p90</span>. It means 90% of requests were 150ms or faster.
</div>

<div v-click="3" class="mt-4">
Median: Take the middle value—average of <code v-mark.green="4">120</code> and <code v-mark.green="4">125</code> = **<code v-mark.green.bold="4">122.5ms</code>**.
</div>

<div v-click="5" class="mt-6 text-lg italic">
Why this rocks: Sorting ignores outliers, showing what most users actually get.
</div>

<!--
This slide uses Magic Move to animate the sorting process!
Clicks:
1: Show P90 text
2: Highlight P90 value and explanation
3: Show Median text
4: Highlight Median values
5: Show "Why this rocks..."
-->

---
layout: two-cols
layoutClass: gap-8
level: 1
---

# Why Percentiles Rule in DevOps

<div v-click="1">
- **Real user experience**: Percentiles (e.g., p90, p95) focus on what most users see, not rare spikes.
</div>
<div v-click="2">
- **SLOs and SLAs**: You set targets like "95% of requests < 200ms." Percentiles measure this directly.
</div>
<div v-click="3">
- **Outlier resistance**: A few slow requests don't ruin your metrics.
</div>

::right::

<div v-click="4" class="mt-8">
### Compare (from our first example):
<br>
Average: <code v-mark.red="5">**399.5ms**</code>
<p>(Suggests poor performance)</p>
<br>
p90: <code v-mark.green="5">**150ms**</code>
<p>(Shows most users are fine)</p>
</div>

<div v-click="6" class="mt-6">
<mermaid caption="Average vs P90 visualization from first example" alt="Bar chart showing average much higher than P90">
graph LR
    A[Average: 399.5ms] -->|Skewed by Outlier| BadExperience(Poor Perception)
    P[p90: 150ms] -->|Represents Majority| GoodExperience(Actual Perception)

    style A fill:#fecaca,stroke:#ef4444,stroke-width:2px
    style P fill:#d1fae5,stroke:#10b981,stroke-width:2px
</mermaid>
</div>

<!--
This slide uses:
- two-cols layout for a clear comparison.
Clicks:
1: Show "Real user experience"
2: Show "SLOs and SLAs"
3: Show "Outlier resistance"
4: Show "Compare" text
5: Highlight comparison values
6: Show Mermaid diagram
-->

---
level: 2
title: Example 1 - Bad Average, Good Percentiles
---

# When Averages Fail
## Example 1: Bad Average, Good Percentiles

Scenario: 10 requests: `50, 60, 55, 65, 70, 75, 80, 85, 90,` <code v-mark.bold.red>5000</code>

<div grid="~ cols-3 gap-4" class="mt-4">
  <div v-click="1">
    **Average:**
    ```ts
    (50+60+55+65+70+75+80+85+90+5000)/10
    // = 563ms
    ```
    <span v-mark.red="2">Highly misleading!</span>
  </div>
  <div v-click="3">
    **Median:**
    Sorted: `50,55,60,65,`**`70,75`**`,80,85,90,5000`
    ```ts
    (70+75)/2
    // = 67.5ms
    ```
  </div>
  <div v-click="4">
    **p90:**
    9th value in sorted list
    ```ts
    // = 90ms
    ```
    <span v-mark.green="5">Much better!</span>
  </div>
</div>

<div v-click="6">
  <p class="mt-6 text-lg italic">
Takeaway: One terrible request (<code v-mark.red="7">5000ms</code>) inflates the average, but 90% of users got <90ms. Percentiles tell the true story.
  </p>
</div>

<!--
Clicks:
1: Show Average calculation
2: Highlight "Highly misleading"
3: Show Median calculation
4: Show P90 calculation
5: Highlight "Much better"
6: Show Takeaway text
7: Highlight outlier in takeaway
-->

---
level: 2
title: Example 2 - Good Average, Bad Percentiles
---

# Another Trap
## Example 2: Good Average, Bad Percentiles

Scenario: 10 requests: `50, 50, 50, 50, 50,` <code v-mark.bold.orange>400, 400, 400, 400, 400</code>

<div grid="~ cols-3 gap-4" class="mt-4">
  <div v-click="1">
    **Average:**
    ```ts
    (50*5 + 400*5)/10
    // = 225ms
    ```
    <span v-mark.yellow="2">Seems okay...</span>
  </div>
  <div v-click="3">
    **Median:**
    Sorted: `50,50,50,50,`**`50,400`**`,400,400,400,400`
    ```ts
    (50+400)/2
    // = 225ms
    ```
  </div>
  <div v-click="4">
    **p90:**
    9th value in sorted list
    ```ts
    // = 400ms
    ```
    <span v-mark.red="5">Alarming!</span>
  </div>
</div>

<div v-click="6">
  <p class="mt-6 text-lg italic">
Takeaway: Average looks decent, but <span v-mark.highlight.orange="7">50% of users waited 400ms!</span> Percentiles expose the pain for many users.
  </p>
</div>

<!--
Clicks:
1: Show Average calculation
2: Highlight "Seems okay..."
3: Show Median calculation
4: Show P90 calculation
5: Highlight "Alarming!"
6: Show Takeaway text
7: Highlight issue in takeaway
-->

---
level: 2
title: Example 3 - Averages and Percentiles Align
---

# When Averages and Percentiles Align
## Example 3

Scenario: 10 requests: `100, 102, 104, 106, 108, 110, 112, 114, 116, 118`

<div grid="~ cols-3 gap-4" class="mt-4">
  <div v-click="1">
    **Average:**
    ```ts
    // Sum / 10 = 109ms
    ```
  </div>
  <div v-click="2">
    **Median:**
    ```ts
    // (108+110)/2 = 109ms
    ```
  </div>
  <div v-click="3">
    **p90:**
    ```ts
    // 9th value = 116ms
    ```
  </div>
</div>

<div v-click="4">
  <p class="mt-6 text-lg italic">
Takeaway: No significant outliers, so average, median, and percentiles align. <span v-mark.highlight.green="5">But this is rare in real systems!</span>
  </p>
</div>

<div
  v-motion
  :initial="{ x: 100, opacity: 0 }"
  :enter="{ x: 0, opacity: 1, transition: { delay: 1000 } }"
  class="mt-4 p-2 bg-teal-50 text-teal-700 rounded"
>
This scenario represents a very consistent system performance, which is ideal but not always the reality.
</div>


<!--
Clicks:
1: Show Average
2: Show Median
3: Show P90
4: Show Takeaway text
5: Highlight takeaway point
The v-motion element is not click-controlled in this sequence.
-->

---
layout: default
level: 1
---

# Actionable Takeaways for DevOps

<div class="grid grid-cols-1 md:grid-cols-2 gap-6 mt-6">
  <div v-click="1" class="p-4 border rounded-lg shadow hover:shadow-xl transition-shadow">
    <h3 class="text-xl font-semibold text-emerald-600">🎯 Use Percentiles</h3>
    Use percentiles (p90, p95, p99) for monitoring and SLOs. They reflect actual user experience.
  </div>

  <div v-click="2" class="p-4 border rounded-lg shadow hover:shadow-xl transition-shadow">
    <h3 class="text-xl font-semibold text-amber-600">⚠️ Avoid Averages (Mostly)</h3>
    Avoid averages for response times unless data is very consistent (rare in production).
  </div>

  <div v-click="3" class="p-4 border rounded-lg shadow hover:shadow-xl transition-shadow">
    <h3 class="text-xl font-semibold text-sky-600">📈 Example SLO</h3>
    Set an SLO: "95% of API requests < 200ms." Track p95 to ensure you meet it.
    <br>
    <span class="text-sm text-gray-600">e.g. $\\text{P}_{95}(\\text{response time}) < 200\\text{ms}$</span>
  </div>

  <div v-click="4" class="p-4 border rounded-lg shadow hover:shadow-xl transition-shadow">
    <h3 class="text-xl font-semibold text-purple-600">🛠️ Tool Tip</h3>
    Use Prometheus, Grafana, or Datadog to track percentiles easily. They have built-in functions!
    <br>
    <span class="text-sm text-gray-600">(e.g., `histogram_quantile()` in PromQL)</span>
  </div>
</div>

<div v-click="5">
  <p class="mt-12 text-2xl text-center font-semibold text-gray-700">
Final word: <span class="text-green-500">Percentiles empower you</span> to focus on what matters—<span class="text-blue-500">your users!</span>
  </p>
</div>

<!--
Clicks:
1: Show "Use Percentiles" card
2: Show "Avoid Averages" card
3: Show "Example SLO" card
4: Show "Tool Tip" card
5: Show "Final word"
-->

---
title: Table of Contents
level: 1
---

# Table of Contents

You can use the `Toc` component to generate a table of contents for your slides.
The `level` in each slide's frontmatter determines its depth in the ToC.

```html
<Toc minDepth="1" maxDepth="2" />
```

<Toc class="mt-4" minDepth="1" maxDepth="2" />

<!--
This slide demonstrates the Table of Contents feature.
- Make sure slides that should appear in ToC have `level` set in their frontmatter.
No click animations on this slide.
-->

---
layout: two-cols
title: More Slidev Features
dragPos:
  logo: 0,-118,0,0
---

# Bonus: Slidev's Powerhouse Features

Slidev offers much more! Here are a few from the sample:

- **LaTeX**: For beautiful math equations.
  Inline: $\\sqrt{3x-1}+(1+x)^2$
  Block:
  $$
  \\begin{aligned}
  \\nabla \\cdot \\vec{E} &= \\frac{\\rho}{\\varepsilon_0} \\\\
  \\nabla \\cdot \\vec{B} &= 0 \\\\
  \\nabla \\times \\vec{E} &= -\\frac{\\partial\\vec{B}}{\\partial t} \\\\
  \\nabla \\times \\vec{B} &= \\mu_0\\vec{J} + \\mu_0\\varepsilon_0\\frac{\\partial\\vec{E}}{\\partial t}
  \\end{aligned}
  $$

- **Diagrams**: Create diagrams from text.
  ```mermaid
  graph TD
      A[Start] --> B{Decision};
      B -- Yes --> C[Good Percentiles!];
      B -- No --> D[Check Outliers!];
  ```

::right::

- **Motions**: Powered by `@vueuse/motion`.
  ```html
  <div v-motion :initial="{ x: -80 }" :enter="{ x: 0 }">
    Animated Text!
  </div>
  ```
  <div
    v-motion
    :initial="{ x: -80, opacity: 0 }"
    :enter="{ x: 0, opacity: 1, transition: { delay: 500 } }"
    class="text-2xl text-blue-500 mt-4"
  >
    Animated Text!
  </div>

- **Draggable Elements**: (Double-click to move)
  ```md
  <img v-drag="'logo'" src="https://sli.dev/logo.png" class="w-20">
  ```
  <img v-drag="'logo'" src="https://sli.dev/logo.png" class="w-20 cursor-grab active:cursor-grabbing">

- **Monaco Editor**: For live coding (add `{monaco}` to code block).
  ```ts {monaco}
  // Try editing this code!
  function greet(name: string) {
    console.log(\`Hello, \${name}!\`);
  }
  greet('Developer');
  ```

<!--
This slide showcases some extra Slidev features as requested.
- LaTeX for math.
- Mermaid for diagrams.
- v-motion for animations.
- v-drag for draggable elements.
- Monaco editor for interactive code blocks.
No sequential v-click animations on this slide.
-->

---
layout: center
class: text-center
---

# Learn More About Slidev

[Documentation](https://sli.dev) · [GitHub](https://github.com/slidevjs/slidev) · [Showcases](https://sli.dev/resources/showcases)

Explore themes, create custom components, and much more!

<PoweredBySlidev mt-10 />

<!--
Final "Learn More" slide.
- Links to official Slidev resources.
- PoweredBySlidev component.
No click animations on this slide.
-->
